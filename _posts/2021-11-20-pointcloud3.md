---
layout: post
title: PCL Clusterting 방법 주석
category: Machine Vision
tag: Machine Vision
---

```c++
void segment(std::vector<int>& labels, std::vector<std::vector<int>>& label_indices)
{
    std::vector<int> run_ids;

    std::vector<int> first_indices;

    labels.resize(m_size); // number of pointcloud, we will label evety single point
    std::fill(labels.begin(), labels.end(), INVALID_VAL);
    std::fill(m_valid_mask.begin(), m_valid_mask.end(), INVALID_VAL); //used for neibor point exist or not.

    bool first_cell_found = false;

    int clust_id = 0;

    // first cloud indices unknown. then
    if ( (*m_cloud_indices)[0] == PointTypeVal::UNKNOWN )
    {
        labels[0] = clust_id++;
        run_ids.push_back(labels[0]);
        first_indices.push_back(0);
        m_valid_mask[0] = 1;
        first_cell_found = true;
    }


    // initialize coloms at first row
    for (int colIdx = 1; colIdx < m_width; ++colIdx)
    {
        // skip it if it is nonvalid.   
        if ((*m_cloud_indices)[colIdx] != PointTypeVal::UNKNOWN) //(*m_ndt_cell_ptr)[colIdx].type == NdtCellType::CORNER ||
            continue;


        // mark it the point is valid.
        m_valid_mask[colIdx] = 1;

        // if first cell not found it, which mean 3 row is started not firs row in matrix according to making normal vector

        /*
        Normal vector에 대한 matrix는 이렇게 됨.,
        NaN,    NaN,   NaN,  NaN,NaN,NaN,NaN
        NaN,    NaN,   NaN,  NaN,NaN,NaN,NaN
        valid, valid, valid, valid, NaN, NaN
        valid, valid, valid, valid, NaN, NaN
        valid, vaild, vaild, valid, NaN, NaN
        valid, valid, valid, valid, NaN, NaN
        */

        // 영억에 대한 노마 벡터는 처음 기준으로 잡고는 포인터의 인덱스 위치이다.

        if (!first_cell_found)
        {
            labels[colIdx] = clust_id++;
            run_ids.push_back(labels[colIdx]); // every each point label stored it in run_ids.
            first_indices.push_back(colIdx); // remember first indices because of remembering starting point as form as above.
            first_cell_found = true;
        }

        // If the neighboring pixel is invalid skip which mean boundary
        if (m_valid_mask[colIdx - 1] == -1) continue;

        // Compare with right neighbor
        // 노마 벡터끼리 curvature와 distance를 비교를 하여서 same group인지 확인한다.
        if (m_plane_comparator->compare(colIdx, colIdx - 1))
        {
            // The two pixels are in the same plane
            labels[colIdx] = labels[colIdx - 1];
        }

        // if not match above follwing above condition, then add cluster id
        if (labels[colIdx] == INVALID_VAL)
        {
            labels[colIdx] = clust_id++;
            run_ids.push_back(labels[colIdx]); // nubmer of cluster.
            first_indices.push_back(colIdx); // new cluster id index

        }
    }

    // each row comparing.
    unsigned int cur_row = m_width;
    unsigned int prev_row = 0;
    for (int rowIdx = 1; rowIdx < m_height; ++rowIdx, prev_row = cur_row, cur_row += m_width)
    {
        // colum at first row
        if ((*m_cloud_indices)[cur_row] == PointTypeVal::UNKNOWN) // (*m_ndt_cell_ptr)[cur_row].type != NdtCellType::CORNER &&
        {
            // if ((*m_ndt_cell_ptr)[cur_row].curvature <= m_max_curvature)
            {
                m_valid_mask[cur_row] = 1;

                // when above algorithm haven found the firscell
                if (!first_cell_found)
                {
                    labels[cur_row] = clust_id++;
                    run_ids.push_back(labels[cur_row]);
                    first_indices.push_back(cur_row);
                    first_cell_found = true;
                }

                // 위아래 compare
                if (m_valid_mask[prev_row] != -1)
                {
                    if (m_plane_comparator->compare(cur_row, prev_row))
                    {
                        // if (m_plane_comparator->compare(cur_row, first_indices[labels[prev_row]], m_angular_thresh - 0.01, m_distance_thresh + 0.03))
                        {
                            labels[cur_row] = labels[prev_row];
                        }
                    }

                    if (labels[cur_row] == INVALID_VAL)
                    {
                        labels[cur_row] = clust_id++;
                        run_ids.push_back(labels[cur_row]);
                        first_indices.push_back(cur_row);

                    }
                }
            }
        }

        // For the rest of the row
        /*
        즉,
        a b
        c d
        의 행렬이 있다면,

        c d
        a c 로 위아래로 비교해서 cluster를 한다.
        */
        for (int colIdx = 1; colIdx < m_width; ++colIdx)
        {
            int idx = cur_row + colIdx;
            int idx_up = prev_row + colIdx;
            // if point labeled in novalid
            if ((*m_cloud_indices)[idx] != PointTypeVal::UNKNOWN) //(*m_ndt_cell_ptr)[idx].type == NdtCellType::CORNER ||
                continue;


            // start from valid data.
            m_valid_mask[idx] = 1;
            if (!first_cell_found)
            {
                labels[idx] = clust_id++;
                run_ids.push_back(labels[idx]);
                first_indices.push_back(idx);
                first_cell_found = true;
            }

            // if there is neibor. colum neibor
            if (m_valid_mask[idx - 1] != -1)
            {    
                // Compare with left
                //
                if (m_plane_comparator->compare(idx, idx - 1))
                {                       
                    labels[idx] = labels[idx - 1];
                }
            }

            if (m_valid_mask[idx_up] != -1)
            {    
                if (m_plane_comparator->compare(idx, idx_up))
                {
                    // if (m_plane_comparator->compare(idx, first_indices[labels[idx_up]], m_angular_thresh - 0.01, m_distance_thresh + 0.03))
                    {
                        // if label has not marked row and colum neigbor before, then we make same label ubove one.
                        if (labels[idx] == INVALID_VAL)
                        {
                            labels[idx] = labels[idx_up];
                        }
                        else
                        // otherwise, label already put as same as neibor label(cluster id) by comparison function, then, we need to choose
                        // which one more close to up neibor or left neibor of current point.
                        {
                            // find root algorithm
                            // cluster id가 저장된 run_ids에 cluster가 시작된 초기 포인트 id를 검색을 한다.
                            int root_left = findRoot(run_ids, labels[idx]); // 클러스터가 시작되는 첫 id, 즉 cluster id라고도 말할 수 있음
                            int root_up   = findRoot(run_ids, labels[idx_up]); // 클러스터가 시작되는 첫 id, 즉 cluster id라고도 말할 수 있음

                            // Choose the smaller index
                            // 즉 위쪽을 하든, 왼쪽을 하든 결정하게 된다.
                            if (root_left < root_up)
                            {
                                // root_up cluster id
                                // root_left cluster id
                                // run_ids[root_up] : output, cluster id
                                // root_up : label[index] = cluster id
                                // index가 작은걸로 멀지한다.
                                // 즉 왼쪽걸로 할지, 오른쪽걸로 할지 결정
                                run_ids[root_up] = root_left; /
                            }
                            else
                            {
                                run_ids[root_left] = root_up;
                            }
                        }
                    }
                }
            }
            // The current pixel does not belong to either up or left pixel
            if (labels[idx] == INVALID_VAL)
            {
                labels[idx] = clust_id++;
                run_ids.push_back(labels[idx]);
                first_indices.push_back(idx);

            }

        }
    }
    // done labeling idx
    //

    if (clust_id == 0) return; // Nothing was found

    // 왜 이것을 사용하는가?
    // 클러스트 아이디 수만큼 맵핑한다.
    std::vector<int> map(clust_id);
    int max_id = 0;
    // cluster id를 mapping 시킨다.
    for (int runIdx = 0; runIdx < run_ids.size(); ++runIdx)
    {
        // 만약 순차적인 클러스터 아이디와, run_ids[runIdx]가 같다면, 즉 순차적으로 같다면
        if (run_ids[runIdx] == runIdx)
        {
            // map에도 순차적으로 넣음
            map[runIdx] = max_id++;
        }
        else
        {
            // 만약 순차적으로 런인덱스가 다르다면,
            // idx의 루트를 찾는다.
            // map runidx를 map root index로 바꾼다.
            map[runIdx] = map[findRoot(run_ids, runIdx)];
        }
    }

    // label indices.
    // mapping된 label[idx] 정보를 label_indices에다가 클러스터링 id에 관련된 point를 저장한다.
    label_indices.resize(max_id + 1);
    for (int idx = 0; idx < m_size; idx++)
    {
        // only valid label read.
        if (labels[idx] != INVALID_VAL)
        {
            labels[idx] = map[labels[idx]]; // map cluster id to real label[idx]
            label_indices[labels[idx]].push_back(idx); // cluster id, put point index
        }
    }

}

```
