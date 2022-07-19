![image-20220719200848506](https://devil-picture-bed.oss-cn-shenzhen.aliyuncs.com/image/202207192008638.png)



## 1.冒泡排序：

时间复杂度：O($n^{2}$)，最好情况O($n$)，最坏情况O($n^{2}$)

空间复杂度：O($1$)

稳定性：稳定

```c++
/***   提前结束版本   ***/
vector<int> bubbleSort(vector<int> &data)
{
    int n = data.size();
    bool flag = true;   // 添加flag，当前轮次没有交换则不进行下一轮直接返回
    for (int i = 0; i < n - 1; i++){            // 外层控制轮数
        flag = true;
        for (int j = 0; j < n - i - 1; j++){    // 内层控制交换数量
            if (data[j] > data[j + 1]){
                flag = false;
                std::swap(data[j], data[j + 1]);     // 交换
            }
        }
        if(flag) return data;    // flag为真说明当前轮没有发生交换
    }
    return data;
}
-----------------------------------------------------------------------
/***   提前结束 + 冒泡边界优化   ***/
// 记录上一轮交换的最后一次边界
vector<int> bubbleSort(vector<int> &data)
{
    int n = data.size();
    int lastIndex = n - 1;  // 初始化冒泡边界**
    int swapIndex = -1;
    bool flag = false;   // 添加flag，当前轮次没有交换则不进行下一轮直接返回
    while(flag == false){  // 外层 保证没有交换的时候退出循环
        flag = true;
        // 每轮循环，通过依次向右比较两个数，将本轮循环中最大的数放到最右
        for (int j = 0; j < lastIndex; j++){    // 内层 控制交换
            if (data[j] > data[j + 1]){
                flag = false;
                std::swap(data[j], data[j + 1]);     // 交换
                swapIndex = j;     // 每次交换，记录未排序的最后位置
            }
        }
        lastIndex = swapIndex; // 每轮交换结束 把未排序最后位置赋值
    }
    return data;
}
```



## 2.选择排序：

时间复杂度：O($n^{2}$)，最好情况O($n^{2}$)，最坏情况O($n^{2}$)

空间复杂度：O($1$)

稳定性：不稳定

```c++
/***   单元选择排序   ***/
vector<int> selectSort(vector<int> &data)
{
    int n = data.size();
    for (int i = 0; i < n - 1; i++){            // 外层控制轮数
        int minIndex = i;
        // 找到本轮执行中最小的元素，将最小值下标赋值给min
        for (int j = i + 1; j < n; j++){        // 内层控制最小数值索引选择
            if (data[j] < data[minIndex]){
                minIndex = j;                 // 寻找当前轮最小值索引
            }
        }
        // 若本轮第一个数字不是最小值，则交换位置
        if(minIndex != i) std::swap(data[i], data[minIndex]);       
    }
    return data;
}
-----------------------------------------------------------------------
/***   双元选择排序   ***/ 
vector<int> selectSort(vector<int> &data)
{
    int n = data.size();
    // 每轮确定两个数字，因此界也会动态变化
    for (**int i = 0; i < n - 1 - i; i++**){        // 外层控制轮数
        int minIndex = i, maxIndex = i;
        // 找到本轮执行中最小的元素，将最小值下标赋值给min
        for (int j = i + 1; j < n - i; j++){    // 内层控制最小数值索引选择
            if (data[j] < data[minIndex]){
                minIndex = j;                   // 寻找当前轮最小值索引
            }
            if (data[j] > data[maxIndex]){
                maxIndex = j;                   // 寻找当前轮最大值索引
            }
        }
        // 若本轮最大值等于最小值，说明未排序部分所有元素相等，无需再排序
        if (minIndex == maxIndex) return data;
        
        // 若本轮第一个数字不是最小值，则交换位置
        if(minIndex != i) std::swap(data[i], data[minIndex]);
        
        // 在交换i和minIdx时，有可能出现i即maxIdx的情况
        // 此时需要修改maxIdx为minIdx（即原来的i 所在的值）
        if(maxIndex == i) maxIndex = minIndex;
        
        // 若本轮第一个数字不是最大值，则交换位置（将最大值与本轮最后一个数字交换位置）
        if(maxIndex != i) std::swap(data[n - 1 - i], data[maxIndex]);
    }
    return data;
}
```



## 3.插入排序：

时间复杂度：O($n^{2}$)，最好情况O($n$)，最坏情况O($n^{2}$)

空间复杂度：O($1$)

稳定性：稳定

```c++
/***   直接插入排序   ***/   
vector<int> insertSort(vector<int> &data)
{
    int n = data.size();
    // n - 1 轮次执行
    for (int i = 1; i < n; i++){ 
        int curNum = data[i];
        int pre_index = i - 1;
        // 寻找 当前数值 data[i] 需要插入的位置的前一个位置 pre_index
        // pre_index 后面的数都已经后移一个位置了
        while (pre_index >= 0 && data[pre_index] > curNum){
            data[pre_index + 1] = data[pre_index];
            pre_index--;
        }
        if (pre_index != i - 1) data[pre_index + 1] = curNum;
    }
    return data;
} 
-----------------------------------------------------------------------
/***   折半插入排序  ***/
// 插入位置：采用二分查找方式
// 二分查找第一个小于等于 当前元素的 位置
vector<int> insertSort(vector<int> &data)
{
    int n = data.size();
    // n - 1 轮次执行
    for (int i = 1; i < n; i++){
        // 若当前插入对象 大于等于 前一个对象，无需插入
        if (data[i] >= data[i-1]) continue; 
        int curNum = data[i];
        
        // 二分查找： 查找 小于等于 当前插入对象 的 最后一个位置
        //               也就是大于当前插入对象的 第一个位置，在该位置插入
        int left = 0, right = i - 1;
        while (left < right){
            int mid = left + ((right - left) >> 1);
            if (data[mid] > curNum) right = mid;
            else if (data[mid] <= curNum) left = mid + 1;
        }
        
        for (int j = i; j > left; j--){  // 移动
            data[j] = data[j - 1];
        }
        data[left] = curNum;   // 插入
    }
    return data;
} 
```



## 4.希尔排序：

时间复杂度：O($nlogn$)，最好情况O($nlog^2n$)，最坏情况O($nlog^2n$)

空间复杂度：O($1$)

稳定性：不稳定

```c++
vector<int> shellSort(vector<int> &data)
{
    int n = data.size();
    int gap = n / 2; // 初始增量定义
    while (gap > 0){   // gap 最小为1，此时即为直接插入排序
        for (int i = gap; i < n; i++){   // 从当前所有序列的第二个数开始
            int curNum = data[i];
            int pre_index = i - gap; // 当前序列的前一个数
            // 寻找 当前序列的当前数值 data[i] 的插入位置
            // pre_index 后面 每隔gap的数值 都已经后移
            while (pre_index >= 0 && data[pre_index] > curNum){
                data[pre_index + gap] = data[pre_index];
                pre_index -= gap;
            }
            if (pre_index != i - gap) data[pre_index + gap] = curNum;
        }
        gap /= 2; // 每次增量缩小一倍，直到1为止
    }
    return data;
    // 这里的 while(gap > 0) 可以改写为
    // for(gap = n/2; gap > 0; gap /= 2){
    // }
}
```



## 5.归并排序：

时间复杂度：O($nlogn$)，最好情况O($nlogn$)，最坏情况O($nlogn$)

空间复杂度：O($n$)

稳定性：稳定

```c++
/***   递归版本：自顶向下 非原地归并   ***/
void mergeSort(vector<int> &data, int left, int right)
{
    if (left == right)
        return;
    int mid = left + ((right - left) >> 1);
    mergeSort(data, left, mid);
    mergeSort(data, mid + 1, right);
    merge(data, left, mid, right);
}
void merge(vector<int> &data, int left, int mid, int right)
{
    int i = left, j = mid + 1; // 左部分起始位置，右部分起始位置
    vector<int> tmp(right - left + 1);
    int Index = 0;
    while(i <= mid && j <= right){
        if(data[i] <= data[j]) tmp[Index++] = data[i++];
        else tmp[Index++] = data[j++];
    }
    // 左半部分有剩余 直接添加到tmp
    while (i <= mid) tmp[Index++] = data[i++];
    // 右半部分有剩余 直接添加到tmp
    while (j <= right) tmp[Index++] = data[j++];

    // **将tmp拷贝回data**
    for (int i = left,Index = 0; i <= right; i++)
        data[i] = tmp[Index++];
}


-----------------------------------------------------------------------
/***   非递归版本：自底向上 非原地归并   ***/
void mergeSort(vector<int> &data)
{
    int n = data.size();
    // 间隔，注意不能写成gap < arr.length / 2 + 1
    // 此种写法只适用于元素个数为2的n次幂时，此时刚好最后一次两相等部分合并
    for (int gap = 1; gap < n; gap *= 2){
        // 基本分区合并(随着间隔的成倍增长，一一合并，二二合并，四四合并...)
        // left <= n - 1 - gap; 保证最后一个left 有两个gap的元素进行合并，
        // 然后 left 向后移动 2*gap
        for (int left = 0; left <= n - 1 - gap; left += 2 * gap){
            // 调用非原地合并方法。
            // 左边界 = left+gap-1; 右边界= left+2*gap-1;
            merge(data, left, left+gap-1, min(n-1, left+2*gap-1));
        }
    }
}
void merge(vector<int> &data, int left, int mid, int right)
{
    int i = left, j = mid + 1; // 左部分起始位置，右部分起始位置
    vector<int> tmp(right - left + 1);
    int Index = 0;
    while(i <= mid && j <= right){
        if(data[i] <= data[j]) tmp[Index++] = data[i++];
        else tmp[Index++] = data[j++];
    }
    // 左半部分有剩余 直接添加到tmp
    while (i <= mid) tmp[Index++] = data[i++];
    // 右半部分有剩余 直接添加到tmp
    while (j <= right) tmp[Index++] = data[j++];

    // 将tmp拷贝回data
    for (int i = left,Index = 0; i <= right; i++)
        data[i] = tmp[Index++];
}
```



## 6.快速排序：

时间复杂度：O($nlogn$)，最好情况O($nlogn$)，最坏情况O($n^2$)

空间复杂度：O($logn$)

稳定性：不稳定

```c++
/***   普通版本：递归快排   ***/
// 随机选择 pivot 元素快排
void quickSort(vector<int>& data, int left, int right){
    if(left >= right) return;
    int pivot = partition(data, left, right);
    quickSort(data, left, pivot - 1);
    quickSort(data, pivot + 1, right);
}
// 一次划分
int partition(vector<int>& data, int left, int right){
    int randIndex = rand() % (right - left + 1) + left;
    swap(data[randIndex], data[left]); // 交换到data[left];
    int pivotNum = data[left];
    ---------------------------------------------------------------
    
    // 基准选left，就先找 right，选right，就先找left
    while(left < right){
        // 从右边找第一个小于 data[pivot] 的值 然后赋值到左边空位
        while(left < right && data[right] >= pivotNum) right--;
        if(left < right) data[left++] = data[right];

        // 从左边找第一个大于 data[pivot] 的值 然后赋值到右边空位
        while(left < right && data[left] <= pivotNum) left++;
        if(left < right) data[right--] = data[left];
    }
    // 退出循环 left == right，将提取的 pivot 赋值到相应位置
    data[left] = pivotNum;
    return left;        // 此时 left == right
    
    ---------------------------------------------------------------
    
    // 上述划线部分存在多种写法
    /***   // 单指针扫描划分  p1 作为扫描指针
    int pivot = left, p1 = left + 1, p2 = right;
    while(p1 <= p2){
        if(data[p1] <= data[pivot])
            p1++;       // 找到第一个比基准大的，然后和p2交换
        else{
            // 交换之后，p2 向前移动到下一个 放置比基准大的位置
            swap(data[p1], data[p2--]);
        }
    } 
    swap(data[pivot], data[p2]);  // 可以画草图了解下p2即最后的分界点，放置基准元素即可
    return p2;
    ***/
    
    ---------------------------------------------------------------
     
    /***   // 双指针扫描划分
    int pivot = left, p1 = left + 1, p2 = right;
    while(p1 <= p2){
        // 注意  条件里面 要加上 p1 <= p2;
        while(p1 <= p2 && data[p1] <= data[pivot]) p1++;
        while(p1 <= p2 && data[p2] >= data[pivot]) p2--;
        if(p1 < p2) swap(data[p1], data[p2]);
    } 
    swap(data[pivot], data[p2]);  // 可以画草图了解下p2即最后的分界点，放置基准元素即可
    return p2;
    ***/
    
    ---------------------------------------------------------------
        
    /***   // 一次扫描划分
    // p1为存放小于基准元素的前一个位置，p2为扫描指针
    int pivot = left, p1 = left, p2 = p1 + 1;
    while (p2 <= right)
    {
        // 对于小于基准的元素，先把p1 后移，然后交换到 p1的位置，
        // 保证p1 永远都是小于基准的最后一个元素
        if (data[p2] < data[pivot]) swap(data[p2], data[++p1]);
        p2++;
    }
    // 跳出循环 则交换 p1 和基准元素，因为p1为小于基准的元素，因此可以保证划分成功
    swap(data[p1], data[pivot]);
    return p1;
    ***/
}

/***   集合版本：递归快排   ***/

void quickSort(vector<int>& data, int left, int right){
    if(left >= right) return;

    int randomInex = rand() % (right - left + 1) + left;
    swap(data[randomInex], data[left]);
    int pivotNum = data[left],p1 = left, p2 = right;
    
    ---------------------------------------------------------------
    while(p1 < p2){
        // 从右边找第一个小于 data[pivot] 的值 然后赋值到左边空位
        while(p1 < p2 && data[p2] >= pivotNum) p2--;
        if(p1 < p2) data[p1++] = data[p2];

        // 从左边找第一个大于 data[pivot] 的值 然后赋值到右边空位
        while(p1 < p2 && data[p1] <= pivotNum) p1++;
        if(p1 < p2) data[p2--] = data[p1];
    }
    // 退出循环 left == right，将提取的 pivot 赋值到相应位置
    data[p1] = pivotNum;
    ---------------------------------------------------------------

    quickSort(data, left, p1 - 1);
    quickSort(data, p1 + 1, right);
}
```



## 7.堆排序：

时间复杂度：O($nlogn$)，最好情况O($nlogn$)，最坏情况O($nlogn$)

空间复杂度：O($1$)

稳定性：不稳定

```c++
/***   自上向下调整：大根堆（最终结果从小到大排序）   ***/
// 堆排序 主函数
void heapSort(vector<int>& data){
    int root = 0, endIndex = data.size() - 1;
    if(root >= endIndex) return;
    // 一次就建堆（初始化建堆），多次向下调整   -- 默认最大堆
    **buildHeap(data, root, endIndex);**
    for (int curIndex = endIndex; curIndex >= 0; curIndex--){
        swap(data[root], data[curIndex]);  // 每次交换堆顶元素到最后一个位置
        adjustDown(data, root, curIndex - 1);   //一次向下调整
    }
}
// 初始化建堆
void buildHeap(vector<int>& data, int root, int endIndex){
    int parent = (endIndex - 1) >> 1;  // 从最后一个节点的父节点开始向下调整
    while(parent >= root){           // 循环调整到 根节点
        adjustDown(data, parent, endIndex);
        parent--;
    }
}
// 一次向下调整
void adjustDown(vector<int>& data, int parent, int endIndex){
    int child = (parent << 1) + 1;
    // 将 parent 节点循环向下调整，直到没有孩子节点比 parent 大
    while(child <= endIndex){
        if(child + 1 <= endIndex && data[child + 1] > data[child])
            child++;

        if(data[child] > data[parent]){
            swap(data[child], data[parent]);
            parent= child;
            child = (parent << 1) + 1;
        }else break;
    }
}
```



## 8.计数排序：

时间复杂度：O($n+k$)，最好情况O($n+k$)，最坏情况O($n+k$)

空间复杂度：O($k$)

稳定性：稳定

```c++
/***   适合data元素最大值不大的情况，只适合整数排序，不适合负数元素   ***/

void countSort(vector<int> &data)
{
    int n = data.size();
    if(n < 2) return;
    int minNum = data[0], maxNum = data[0];

    // 寻找 data 元素最大最小值
    for (int i = 0; i < n; i++) {
        if(data[i] < minNum) minNum = data[i];
        if(data[i] > maxNum) maxNum = data[i];
    }

    // 辅助计数空间 申请
    int len = maxNum - minNum + 1;
    vector<int> cnt(len,0);

    // 统计 data 元素计数
    for(const auto& num:data) cnt[num - minNum]++;

    // 还原数组   根据 元素 出现的次数，对 原数组 进行 填充
    int dataIndex = 0;  // 需要填充的 data 下标
    for (int cntIndex = 0; cntIndex < len; cntIndex++){
        while (cnt[cntIndex]-- > 0){
            data[dataIndex++] = cntIndex + minNum;
        }
    }
}
```



## 9.桶排序：

时间复杂度：O($n+k$)，最好情况O($n+k$)，最坏情况O($n^2$)

空间复杂度：O($n+k$)

稳定性：稳定

```c++
/***   计数排序的升级版   ***/

void **bucketSort**(vector<int> &data, int r)
{
    int n = data.size();
    if(n < 2) return;
    int minNum = data[0], maxNum = data[0];

    // 寻找 data 元素最大最小值， 计算每个桶的 数据范围
    for (int i = 0; i < n; i++) {
        if(data[i] < minNum) minNum = data[i];
        if(data[i] > maxNum) maxNum = data[i];
    }
    if (minNum == maxNum) return;   // 所有数据相等
    
    int range = (maxNum - minNum + 1) / r + 1;
    
    // 建立桶 对应的 数据空间， 一个桶最多放置 n 个元素
    vector<vector<int>> buckets(r, vector<int>(n,0));
    
    vector<int> cnt(r, 0);   // 记录当前 桶 要存放数据的位置索引
    
    for (const auto& num:data){
        int index = (num - minNum) / range;
        buckets[index][cnt[index]++] = num;
    }
    
    // 对每个桶扫描 **数据回填**
    int index = 0;
    for(int i = 0; i < r; i++){
        // 对每个桶进行排序 （快排 插入 计数 等）
        if(cnt[i] == 0) continue;
        quickSort(buckets[i],0,cnt[i]-1);
        
        // **拼接桶内数据**
        for(int j = 0; j < cnt[i]; j++){
            data[index++] = buckets[i][j];
        }
    }
}

void quickSort(vector<int>& data, int left, int right){
    if(left >= right) return;

    int randomInex = rand() % (right - left + 1) + left;
    swap(data[randomInex], data[left]);
    int pivotNum = data[left],p1 = left, p2 = right;
    
    while(p1 < p2){
        // 从右边找第一个小于 data[pivot] 的值 然后赋值到左边空位
        while(p1 < p2 && data[p2] >= pivotNum) p2--;
        if(p1 < p2) data[p1++] = data[p2];

        // 从左边找第一个大于 data[pivot] 的值 然后赋值到右边空位
        while(p1 < p2 && data[p1] <= pivotNum) p1++;
        if(p1 < p2) data[p2--] = data[p1];
    }
    // 退出循环 left == right，将提取的 pivot 赋值到相应位置
    data[p1] = pivotNum;

    quickSort(data, left, p1 - 1);
    quickSort(data, p1 + 1, right);
}
```



## 10.基数排序：

时间复杂度：O($n*k$)，最好情况O($n*k$)，最坏情况O($n*k$)

空间复杂度：O($n+k$)

稳定性：稳定

```c++
// 计算 data 元素的最大位数
int maxBits(vector<int> &data)
{
    int maxBit = 0, maxNum = data[0];

    // 找最大数值
    for (auto num : data)
        if (num > maxNum)
            maxNum = num;

    // 计算最大位数
    while (maxNum > 0)
    {
        maxBit++;
        maxNum /= 10;
    }
    return maxBit;
}

// 基数排序主算法
void radixSort(vector<int> &data)
{
    int n = data.size();
    if (n < 2)
        return;
    int maxBit = maxBits(data);

    int radix = 1; // 基数初始化

    for (int i = 0; i < maxBit; i++)
    {
        vector<int> cnt(10, 0);  // 计数器  0~9
        vector<vector<int>> tmp; // 临时空间申请

        // 统计每个桶中的记录数
        for (int j = 0; j < n; j++)
        {
            int k = (data[j] / radix) % 10;
            cnt[k]++;
        }

        // 对 0~9 分配相应的临时空间大小
        for (int j = 0; j < 10; j++)
        {
            tmp.emplace_back(vector<int>(cnt[j], 0));
        }

        // 将 data 元素依次放入 临时空间
        for (int j = n - 1; j >= 0; j--)
        {
            int k = (data[j] / radix) % 10;
            tmp[k][--cnt[k]] = data[j];
        }

        // 将 临时空间 元素依次 收集回 data
        int index = 0;
        for (int j = 0; j < 10; j++)
        {
            for (auto num : tmp[j])
                data[index++] = num;
        }
        **radix *= 10;**
    }
}
```



## 排序算法-比较与稳定：

![image-20220719202910445](https://devil-picture-bed.oss-cn-shenzhen.aliyuncs.com/image/202207192029589.png)

## 排序算法-复杂度（表格版本）：

| 排序算法 | 时间复杂度 | 最好情况     | 最坏情况     | 空间复杂度 | 排序方式  | 稳定性 |
| -------- | ---------- | ------------ | ------------ | ---------- | --------- | ------ |
| 冒泡排序 | O($n^2$)   | O($n$)       | O($n^2$)     | O($1$)     | In-place  | 稳定   |
| 选择排序 | O($n^2$)   | O($n^2$)     | O($n^2$)     | O($1$)     | In-place  | 不稳定 |
| 插入排序 | O($n^2$)   | O($n$)       | O($n^2$)     | O($1$)     | In- place | 稳定   |
| 希尔排序 | O($nlogn$) | O($nlog^2n$) | O($nlog^2n$) | O($1$)     | In- place | 不稳定 |
| 归并排序 | O($nlogn$) | O($nlogn$)   | O($nlogn$)   | O($n$)     | Out-place | 稳定   |
| 快速排序 | O($nlogn$) | O($nlogn$)   | O($n^2$)     | O($logn$)  | In-place  | 不稳定 |
| 堆排序   | O($nlogn$) | O($nlogn$)   | O($nlogn$)   | O($1$)     | In-place  | 不稳定 |
| 计数排序 | O($n+k$)   | O($n+k$)     | O($n+k$)     | O($k$)     | Out-place | 稳定   |
| 桶排序   | O($n+k$)   | O($n+k$)     | O($n^2$)     | O($n+k$)   | Out-place | 稳定   |
| 基数排序 | O($n*k$)   | O($n*k$)     | O($n*k$)     | O($n+k$)   | Out-place | 稳定   |

