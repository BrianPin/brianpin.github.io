---
title: A C++ class for Topological sort
category: [C++, DFS, Topological Sort]
---
```cpp
#ifndef __SORT_DFS_H__
#define __SORT_DFS_H__

#include <iostream>
#include <map>
#include <list>

using namespace std;

// G is your graph type
// T is your node type on the graph
template <typename G, typename T>
class TopologySort
{
    enum color_t {WHITE, GREY, BLACK};
    typedef time_mark_t int;
    static time_mark_t t;

    template <typename T>
    class T_ext {
    public:
        explicit T_ext(const T& t):m_ref(t), m_parent(0), m_color(WHITE), m_enter(0), m_finish(0) {}
        ~T_ext(){}
        T m_ref;
        T_ext *m_parent;
        color_t m_color;
        time_mark_t m_enter;
        time_mark_t m_finish;
        std::list<T_ext*> neighbors;
    };
    static void dfs_visit(std::map<T, T_ext*>& map_ref,
                          T_ext* node_ext,
                          std::list<T> &result)
    {
        t++;
        node_ext->m_enter = t;
        node_ext->m_color = GREY;
        std::list<T_ext*>::iterator iter = node_ext->neighbors.begin();
        while (it != node_ext->neighbors.end())
        {
            T_ext* next_node = *it;
            if (next_node->m_color == WHITE)
            {
                next_node->m_parent = node_ext;
                TopologySort::dfs_visit(map_ref, next_node, result);
            }
            ++ it;
        }
        node_ext->m_finish = ++t;
        result.push_front(node_ext->m_ref);
        node_ext->m_color = BLACK;
    }
public:
    static std::list<T> sort(const G& g)
    {
        //extend the original graph to our data structure
        std::map<T, T_ext*> dup_g;
        std::list<T> result;

        const G::const_iterator iter = g.begin();
        while (iter != g.end())
        {
            T_ext* node = new T_ext(*iter);
            dup_g.insert(std::pair<T, T_ext*>(*iter, node));
            ++iter;
        }

        std::map<T, T_ext*>::iterator map_iter = dup_g.begin();
        while (map_iter != dup_g.end())
        {
            T_ext* node_ext = map_iter->second;
            T node = node_ext->m_ref;
            const T::iterator iter = node.begin();
            // the iterator of each individual node supposed to iterate its neighbors
            while (iter != node.end())
            {
                if (dup_g.find(*iter) != dup_g.end())
                {
                    node_ext->neighbors.push_back(dup_g.find(*iter));
                }
                ++iter;
            }
            ++ map_iter;
        }

        t = 0;
        // Now we have our own graph to do the work
        std::map<T, T_ext*>::const_iterator it = dup_g.begin();
        while (it != dup_g.end())
        {
            T_ext* node_ext = it->second;
            if (node_ext->m_color == WHITE)
            {
                TopologySort::dfs_visit(dup_g, node_ext);
            }
        }
        return result;
    }
};

static time_mark_t TopologySort::t = 0;

#endif
```
