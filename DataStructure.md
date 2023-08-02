## 树链剖分
```
int n, m;
int h[N], e[N], ne[N], w[N], idx;
void add(int a, int b) { e[idx] = b, ne[idx] = h[a], h[a] = idx++; }
// id表示重新标号后对应的下标，nw是对应的值
int id[N], nw[N], cnt;
// 树链剖分五件套
// 子树大小  父节点  重儿子  所在重链的开始节点  当前结点的深度
int sz[N], fa[N], son[N], top[N], dep[N];
struct Node {
  int l, r, size;
  ll add, sum;
} tr[N << 2];
// 处理子树节点数并找到重儿子
void dfs1(int u, int pa, int depth) {
  sz[u] = 1, fa[u] = pa, dep[u] = depth;
  for (int i = h[u]; ~i; i = ne[i]) {
    int j = e[i];
    if (j == pa) continue;
    dfs1(j, u, depth + 1);
    sz[u] += sz[j];
    // 重儿子为子节点中子树节点数最大的那个节点
    if (sz[j] > sz[son[u]]) son[u] = j;
  }
}
// 找重链，即找到每个点所属重链的开始节点，同时将树转化为序列
void dfs2(int u, int st) {
  id[u] = ++cnt, nw[cnt] = w[u], top[u] = st;
  if (!son[u]) return;
  // 优先遍历重儿子
  dfs2(son[u], st);
  for (int i = h[u]; ~i; i = ne[i]) {
    int j = e[i];
    if (j == fa[u] || j == son[u]) continue;
    dfs2(j, j);
  }
}

//... 线段树操作
void build(int u, int l, int r) {
  tr[u] = {l, r, 1, 0, nw[l]};
  if (l == r) return;
  int mid = l + r >> 1;
  build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
  pushup(u);
}
//... 线段树操作

// 树链剖分修改查询操作
// 修改路径
void update_path(int u, int v, int x) {
  while (top[u] != top[v]) {
    if (dep[top[u]] < dep[top[v]]) swap(u, v);
    update(1, id[top[u]], id[u], x);
    u = fa[top[u]];
  }
  if (dep[u] < dep[v]) swap(u, v);
  update(1, id[v], id[u], x);
}
// 查询路径
ll query_path(int u, int v) {
  ll res = 0;
  while (top[u] != top[v]) {
    if (dep[top[u]] < dep[top[v]]) swap(u, v);
    res += query(1, id[top[u]], id[u]);
    u = fa[top[u]];
  }
  if (dep[u] < dep[v]) swap(u, v);
  res += query(1, id[v], id[u]);
  return res;
}
// 修改子树
void update_tree(int u, int x) { update(1, id[u], id[u] + sz[u] - 1, x); }
// 查询子树
ll query_tree(int u) { return query(1, id[u], id[u] + sz[u] - 1); }

void solve() {
  // ...
  // 找重儿子，同时更新节点相关信息
  dfs1(1, -1, 1);
  // 树-->链
  dfs2(1, 1);
  // 线段树
  build(1, 1, n);
  //...
}
```
