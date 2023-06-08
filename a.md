# 排序(sort)
排序，就是按照一種關係做排列，譬如說由小到大、由大到小。
排序有很多方法，著名的就有：氣泡排序、合併排序、選擇排序，基本上不會有人手刻這些排序法，單純只為了排序，因為有函式可以用，那就來看怎麼用。
## sort函式
### 如何排序
```cpp=
int a[5] = {3, 4, 1, 2, 4}
sort(a, a + 5);
```
兩個都要寫要排序的資料，第一個擺包含的起始位置，第二個擺後一個位置，(其實就是擺陣列的長度啦），那還有$std::vector$的用法：
```cpp=
vector<int> v;
sort(v.begin(), v.end());
```
當然也可以由大到小：
```cpp=
vector<int> v;
sort(v.begin(), v.end(), greater<int>());
```
有趣的是內建的有小到大是 $!greater<int>()$，所以一般都沒在寫的。

### 內建的排序機制
其中排序的型態可以有很多種譬如 $pair$，如果pair的兩個形態相同，會有內建的比較方式：會先比較第一個key在比較第二個key。

| key 1 | key 2 | 
| -------- | -------- |
| 1  | 4    |
| 1    | 7     |
| 2   | 3     |
| 4    | 1     |

以上就是$pair<int, int>$ 做完排序的樣子，所以key 1一定是遞增，key 2就很不一定了。
```cpp=
vector<pair<int, int>> v = {{2, 3}, {1, 4}, {4, 1}, {1, 7}};
sort(v.begin(), v.end());
```
### 時間複雜度？
$O(N\log N)$
## 自訂排序
我們常會希望資料要按照一定的方式儲存在資料結構裡，有時候不僅僅是由小到大，甚至裡面放 $struct$ 最好都會寫自定排序。
### 比較函式(cmp)
以由小到大舉例：
```cpp=
vector<int> v;
bool cmp(const int &a, const int &b){
    return a < b;
}
sort(v.begin(), v.end(), cmp);
```
先寫一個布林函式(const跟&可以不用加),接著$return$，裡面就放著你想要的兩個數之排列方式。最後，$sort$的第三項擺$cmp$就好了
### 練習
稍微練習一下：

$給你 𝑁 個正整數，請你將奇數排在前面，偶數排在後面，
如果奇偶相同的數字就小的排在前面。$

先想你想要怎麼排序，奇數放前面就代表我要奇數，所以$cmp$就可以這樣寫：
```cpp=
bool cmp(const int &a, const int &b){
    if(a % 2 != b % 2) return a % 2;
    return a < b;
}
```
基本上如果$cmp$裡面只有$if, else,$ 運算符號，時間複雜度依舊是$O(N\log N)$

有空可以寫寫2023TOI初選的pA...
## struct自訂排序
剛剛前面提及相同型態的資料有內建的排序，但如果我們直接執行以下程式碼：
```cpp=
struct point{
    int x, y;
};
vector<point> v = {{2, 3}, {1, 4}, {4, 1}, {1, 7}};
sort(v.begin(), v.end());
```
你會發現，幹根本執行不了。好，所以怎麼辦，假設我們要排成跟內建方法一樣的排序，有兩種方法：
1. $cmp$ 
```cpp=
struct point{
    int x, y;
};
bool cmp(const point &a, const point &b){
    if(a.x != b.x) return a.x < b.y;
    return a.y < b.y;
}
vector<point> v = {{2, 3}, {1, 4}, {4, 1}, {1, 7}};
sort(v.begin(), v.end(), cmp);
```
2. 自訂比較運算符號
$struct$之所以不能直接sort，是因為兩個$struct$沒有天身賦予它的比較關係，既然沒有，那就自訂吧！
```cpp=
struct point{
    int x, y;
    bool operator<(const point &a) const {
        if(x != a.x) return x < a.x;
        return y < a.y;
    }
};
vector<point> v = {{2, 3}, {1, 4}, {4, 1}, {1, 7}};
sort(v.begin(), v.end());

```
只能小於(<)，不能大於(>)。