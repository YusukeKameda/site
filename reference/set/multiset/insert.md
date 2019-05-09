# insert
* set[meta header]
* std[meta namespace]
* multiset[meta class]
* function[meta id-type]

```cpp
pair<iterator,bool> insert(const value_type& x);               // (1)
pair<iterator,bool> insert(value_type&& y);                    // (2) C++11

iterator insert(iterator position, const value_type& x);       // (3) C++03
iterator insert(const_iterator position, const value_type& x); // (3) C++11

iterator insert(const_iterator position, value_type&& y);      // (4) C++11

template <class InputIterator>
void insert(InputIterator first, InputIterator last);          // (5)

void insert(initializer_list<value_type> init);                // (6)

iterator insert(node_type&& nh);                               // (7) C++17
iterator insert(const_iterator hint, node_type&& nh);          // (8) C++17
```
* pair[link /reference/utility/pair.md]
* initializer_list[link /reference/initializer_list/initializer_list.md]

## 概要
新しく一つの要素(引数 `x`, `y`を使う)、要素のシーケンス(入力イテレータまたは `initializer_list` を使う)または[ノードハンドル](/reference/node_handle/node_handle.md)を挿入することにより、 `set` コンテナを拡張する。

 `set` コンテナは重複した値を許さないため、挿入操作はそれぞれの要素が他のコンテナ内の既存要素と同じ値かどうかをチェックし、同じ要素がすでにあれば挿入されない。`multiset`の場合には、同じ値の要素でも挿入される。


- (1) : 新たな値`x`をコピー挿入する
- (2) : 新たな要素`y`をムーブ挿入する
- (3) : 新たな要素`x`をコピー挿入する。`position`パラメータに適切な挿入位置を指定すれば、高速に挿入できる
- (4) : 新たな要素`y`をムーブ挿入する。`position`パラメータに適切な挿入位置を指定すれば、高速に挿入できる
- (5) : イテレータ範囲`[first, last)`の要素を挿入する
- (6) : 初期化子リスト`init`の要素を挿入する
- (7) : `nh` は空である、または、`(*this).get_­allocator() == nh.get_­allocator()`である。
- (8) : `nh` は空である、または、`(*this).get_­allocator() == nh.get_­allocator()`である。


## 戻り値
- (1), (2) : `first` に新しく挿入された要素またはすでに `multiset` に格納されていた同じ値の要素を指すイテレータを設定する。`second` には、要素が挿入されたときに `true` を、同じ値の要素が存在したときに `false` を設定する。
- (3), (4) : 新しく挿入された要素またはすでに `multiset` に格納されていた同じ値の要素を指すイテレータを返す。
- (5), (6) : なし
- (7), (8) : `nh` が空の場合は終端イテレータ、そうでなければ挿入された要素を指すイテレータ。


## 計算量
- (1), (2) : 対数時間
- (3), (4) : 一般に対数時間だが、`x` または `y` が `position` が指す要素の後に挿入された場合は償却定数時間
- (5), (6) : 一般に N log(size + N)※ だが、`first` と `last` の間がコンテナで使われているものと同じ順序基準に従ってソート済みである場合は線形時間。
    - ※ ここで `N` は `first` と `last` の間の距離であり `size` は挿入前のコンテナの [`size()`](size.md)
- (7) : コンテナのサイズの対数、`O(log(size()))`。
- (8) : 挿入が `hint` の直前の位置に行われた場合、償却定数時間。 そうでなければ、コンテナのサイズの対数。


## 備考
内部的に `multiset` コンテナは、コンストラクト時に指定された比較オブジェクトによって要素を下位から上位へとソートして保持する。 (7), (8) の場合要素は、コピーもムーブもされない。


## 例
```cpp example
#include <iostream>
#include <set>

int main ()
{
  std::multiset<int> c1;
  std::multiset<int> c2;

  c1.insert(10);
  c1.insert(10);
  c1.insert(30);

  std::cout << c1.size() << std::endl;

  c2.insert(c1.begin(), c1.end());
  c2.insert(40);

  std::cout << c2.size() << std::endl;
}
```
* insert[color ff0000]
* size()[link size.md]
* c1.begin()[link begin.md]
* c1.end()[link end.md]

### 出力
```
3
4
```


## 関連項目

| 名前                  | 説明                     |
|-----------------------|--------------------------|
| [`erase`](erase.md) | 要素を削除する           |
| [`find`](find.md)   | 指定したキーで要素を探す |


## 参照
- [N2350 Container insert/erase and iterator constness (Revision 1)](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2007/n2350.pdf)
- [N2679 Initializer Lists for Standard Containers(Revision 1)](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2008/n2679.pdf)
- [Splicing Maps and Sets(Revision 5)](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0083r3.pdf)
    - (7), (8)経緯となる提案文書