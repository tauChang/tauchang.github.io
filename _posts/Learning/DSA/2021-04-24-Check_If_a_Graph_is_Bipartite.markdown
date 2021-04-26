---
title:  "Check if a Graph is Bipartite"
date:   2021-04-24 23:00:00 +0800
categories: Learning DSA
---
Great thanks to [this post from cp-algorithms](https://cp-algorithms.com/graph/bipartite-check.html).

First, the definition of a bipartite graph (from Wikipedia):

>In the mathematical field of graph theory, a **bipartite graph** (or **bigraph**) is a graph whose vertices can be divided into two disjoint and independent sets $$U$$ and $$V$$ such that every edge connects a vertex in $$ U $$ to one in $$V$$. Vertex sets $$U$$ and $$V$$ are usually called the **parts** of the graph. Equivalently, a bipartite graph is a graph that does not contain any odd-length cycles

Note that a bipartite graph does not need to be connected. 

To check whether a graph is bipartite, we use the fact that **a graph is bipartite iff it is 2-colorable**. The following implementation uses DFS to label each visited vertex as either $$ 0 $$ or $$1$$, representing the two parts of a bipartite graph. If a visited vertex is labeled $$x$$, its neighbor is labeled $$x \oplus 1$$. If we encounter a visited vertex during DFS, we check if the visited vertex is assigned to the other part. If not, then the graph is not bipartite.


{% highlight cpp %}
int n, m; // n vertices, m edges
vector< vector<int> > adj;
vector<int> label;
bool isBipartite = true;

void dfs(int u){
    for(auto& v:adj[u]){
        if(label[v] == -1){
            label[v] = !label[u];
            dfs(v);
        } else{
            isBipartite &= (label[v] != label[u]);
        }
    }
}

int main(){
    // Input
    cin >> n >> m;
    adj.resize(n);
    label.resize(n, -1);
    for(int i = 0, u, v; i < m; ++i){
        cin >> u >> v; 
        --u; --v;
        adj[u].push_back(v); adj[v].push_back(u);
    }

    // DFS
    for(int i = 0; i < n; ++i){
        if(label[i] == -1) {
            label[i] = 0; // Arbitrarily assign to one side
            dfs(i);
        }
    }

    // Output
    cout << (isBipartite ? "Yes" : "No");
    return 0;
}
{% endhighlight %}


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
