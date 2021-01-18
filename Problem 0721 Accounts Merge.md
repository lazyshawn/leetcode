## § Problem 721 Accounts Merge
> [账户合并](
https://leetcode-cn.com/problems/accounts-merge/)

给定一个列表 accounts，每个元素 accounts[i] 是一个字符串列表，
其中第一个元素 accounts[i][0] 是 名称 (name)，
其余元素是 emails 表示该账户的邮箱地址。

现在，我们想合并这些账户。
如果两个账户都有一些共同的邮箱地址，则两个账户必定属于同一个人。
请注意，即使两个账户具有相同的名称，它们也可能属于不同的人，
因为人们可能具有相同的名称。
一个人最初可以拥有任意数量的账户，但其所有账户都具有相同的名称。

**Q:** 合并账户后，按以下格式返回账户：
每个账户的第一个元素是名称，其余元素是按顺序排列的邮箱地址。
账户本身可以以任意顺序返回。


### Notes
* 题目实质是判断所有的邮箱地址中那些邮箱地址必定属于同一个人。
可以用并查集实现。

* 同一个邮箱对应的用户是唯一的。
可以使用用户id的索引作为并查集的结点，
用哈希映射保存每个id拥有的邮箱。
连通的id表示属于同一个用户名(username)。

* std::move 函数可以避免不必要的拷贝操作。
std::move是为性能而生，
将对象的状态或者所有权从一个对象转移到另一个对象，
只是状态或所有权的转移，没有内存的搬迁或者内存拷贝。。
参考：[c++11 std::move() 的使用](
https://www.cnblogs.com/yoyo-sincerely/p/8658075.html)

* 并查集是将连通的分量连接在一起，并保存在根节点数组中，
需要额外的数据结构来处理连通后的分量。


**My Solution:** 
```cpp
class Solution {
public:
  // 查找
  int find(std::vector<int>& parents, int i) {
    return parents[i] == i ? i : (parents[i] = find(parents, parents[i]));
  }

  // 合并
  void unify(std::vector<int>& parents, int i, int j) {
    parents[find(parents, parents[i])] = find(parents, parents[j]);
  }

  std::vector<std::vector<std::string>> accountsMerge(std::vector<std::vector<std::string>>& accounts) {
    int len = accounts.size();
    std::vector<int> parents(len);
    std::unordered_map<std::string, int> email2id;

    // 初始化并查集
    for (int i=0; i<len; ++i) {
      parents[i] = i;
    }

    // 遍历数组，将拥有相同邮箱的 id 连通
    for (int id=0; id<accounts.size(); ++id) {
      for (int i=1; i<accounts[id].size(); ++i) {
        auto & e = accounts[id][i];
        if (!email2id.count(e)) {
          email2id[e] = id;
        }
        unify(parents, email2id[e], email2id[accounts[id][1]]);
      }
    }

    // 将连通的邮箱放入账户user映射
    std::unordered_map<int, std::vector<std::string>> user;
    for (auto & [e, uid] : email2id) {
      user[find(parents, uid)].push_back(std::move(e));
    }

    // 排序并返回结果
    std::vector<std::vector<std::string>> res;
    for (auto & [uid,ev] : user) {
      sort(ev.rbegin(),ev.rend());
      // 把用户名加到列表前面
      ev.push_back(accounts[uid][0]);
      reverse(ev.begin(), ev.end());
      res.push_back(move(ev));
    }
    return res;
  }
};
```



