```js
/**
 * 十大基本算法
 * 交换排序： 冒泡排序、快速排序
 * 插入排序： 简单插入排序、希尔排序
 * 选择排序： 简单选择排序、堆排序
 * 归并排序： 二路归并排序、多路归并排序
 * 非比较排序：计数排序、桶排序、基数排序
 * ---------------------------------------
 * 排序选择
 * 若n较小( 如n≤50)， 可采用直接插入或直接选择排序；
 * 若文件初始状态基本有序(指正序)，则应选用直接插入、冒泡或随机的快速排序为宜；
 * 若n较大，则应采用时间复杂度为O(n log n) 的排序方法：快速排序、堆排序或归并排序；
 * 若n较小，考虑稳定性，可以使用：基数排序、计数排序或者桶排序。
 */

/**
 * 1、冒泡算法
 * 利用两两对比交换，每一次循环将最大的数交换至序列尾部
 */
const bubbleSort = (list = [9, 4, 7, 1, 3, 2, 5, 6, 8]) => {
  const arr = [].concat(list)
  // 创建happenChange变量来检测本次循环是否发生交换，未发生交换代表序列排序已经完成
  // 第一层外循环为排序趟数，最大趟数为序列长度减一， 比如序列长度为2，则最大只需比较1趟， 序列长度为3， 则最大只需比较2趟。
  for (let i = 0, happenChange, temp; i < arr.length - 1; i++) {
    // 每趟初始化标志，假设标记为未发生交换
    happenChange = false
    // 每次交换比较从0下标开始
    // 实际序列下标比对应长度小1，则 j < arr.length，其次两两交换循环到两者的第一下标位就行，也避免j + 1 溢出，则j < arr.length - 1
    // 实际当前待排序长度需要减去趟数，一趟必定将一个最大值升至尾部， 则 j < arr.length - 1 - i
    for (let j = 0; j < arr.length - 1 - i; j++) {
      if (arr[j] > arr[j + 1]) {
        temp = arr[j]
        arr[j] = arr[j + 1]
        arr[j + 1] = temp
        happenChange = true
      }
    }
    if (!happenChange) {
      // 未发生交换代表序列排序已经完成
      break
    }
  }
  return arr
}

// console.info(bubbleSort())

/**
 * 二、快速排序
 * 快速排序是从序列中选取一个基准值，然后将小于基准的放左边，大于基准的放右边，再将左边右边按照相同逻辑迭代
 */
const quickSort = (list = [9, 4, 7, 1, 3, 2, 5, 6, 8]) => {
  /**
   * 排序方法
   * @param {Array} arr 待排序序列
   * @param {Number} left 起始下标
   * @param {Number} right 终止下标
   */
  const sort = (arr, left = 0, right = arr.length - 1) => {

    // 如果起始大于等于终止，则排序完成
    if (left >= right) {
      return
    }

    let i = left
    let j = right
    // 先从左向右检查，则取右边第一给值为基准，先从右到左检查，则取左边第一个为基准
    let baseValue = arr[right]
    while (i < j) {
      // 从左向右开始检查，寻找大于基准值的值
      while (i < j && arr[i] <= baseValue) {
        i++
      }
      // 将大于基准值的放在右边，或者此时i = j
      arr[j] = arr[i]
      // 从右向左开始检查，寻找小于基准值的值
      while (j > i && arr[j] >= baseValue) {
        j--
      }
      // 将小于基准值的值放在左边，或者此时i = j
      arr[i] = arr[j]
    }
    // i = j 后，将基准值赋值为 i
    arr[i] = baseValue
    // 小于基准值的左区间迭代 -1绕过基准值
    sort(arr, 0, i - 1)
    // 大于基准值的右区间迭代 +1绕过基准值
    sort(arr, i + 1, right)
  }
  const arr = [].concat(list)
  sort(arr)
  return arr
}

// console.info(quickSort())

/**
 * 三、选择排序
 * 选择排序为一趟一趟的寻找最大（最小）值将其放置最右端（最左）
 * 选择排序是冒泡排序的改进
 */
const selectionSort = (list = [9, 4, 7, 1, 3, 2, 5, 6, 8]) => {
  const arr = [].concat(list)
  // 外层循环控制趟数，至多只需要寻找 length - 1趟
  for (let i = 0, min, temp; i < arr.length - 1; i++) {
    // 设最小值为i
    min = i
    // 从min + 1位开始寻找小于min的值，将其放置i位
    for (let j = min + 1; j < arr.length; j++) {
      if (arr[min] > arr[j]) {
        min = j
      }
    }
    // 将最小值放在i位
    if (min !== i) {
      temp = arr[i]
      arr[i] = arr[min]
      arr[min] = temp
    }
  }
  return arr
}

// console.info(selectionSort())

/**
 * 四、插入排序
 * 插入排序是将序列分成已排序序列和待排序序列，从待排序序列中取值插入到已排序序列的适当位置
 * 取序列第一位为已排序序列
 * 插入排序适合插入场景
 */
const insertSort = (list = [9, 4, 7, 1, 3, 2, 5, 6, 8]) => {
  const arr = [].concat(list)
  // i 代表待插入值下标， 默认下标0 为已排序序列
  for (let i = 1, temp; i < arr.length; i++) {
    // i 之前为已排序序列，j 表示当前比较的已排序序列下标
    // 待插入的值
    temp = arr[i]
    // 前位值
    let j = i - 1
    // 待插入值不断和大于自己的已排序值交换位置，直到 j < 0，或者待插入值arr[i]小于等于已排序值arr[j]
    while (j >= 0 && temp < arr[j]) {
      // 前位后移，继续寻找 直到j < 0 或者 前位值等于或者小于插入值
      arr[j + 1] = arr[j--]
    }
    // 如果直到j < 0 都没有寻找到（等于或者小于）插入值的值，则 j 的后一位为插入位置
    // 前位的后位位插入值
    arr[j + 1] = temp
  }
  return arr
}
// console.info(insertSort())

/**
 * 希尔排序，插入排序的一种改进排序
 */
const metznerSort = (list = [9, 4, 7, 1, 3, 2, 5, 6, 8]) => {
  const arr = [].concat(list)
  const len = arr.length
  let temp
  // 设立分割序号，直到分割序号要为1
  // 第一层循环控制分割序号的迭代
  for (let m = Math.floor(len / 2); m > 0; m = Math.floor(m / 2)) {
    // 第二层控制插入排序的起始位置
    for (let i = 0; i < m; i++) {
      // 等比序列 执行插入排序
      for (let j = i + m; j < len; j = j + m) {
        // 本位 插入值
        temp = arr[j]
        // 前位
        let k = j - m
        // 向前迭代寻找位置 如果前位大于 插入值
        while (k >= 0 && temp < arr[k]) {
          // 前位后移
          arr[k + m] = arr[k]
          // 继续前移 直到 k < 0 （没有和插入值相等的值或者没有比插入值小的值） 此时 k + m为要插入位置
          k -= m
        }
        arr[k + m] = temp
      }
    }
  }
  return arr
}
// console.info(metznerSort())

/**
 * 归并排序
 */
const mergeSort = (list = [9, 4, 7, 1, 3, 2, 5, 6, 8]) => {
  // 迭代对半拆分
  const split = (arr) => {
    if (arr.length < 2) {
      return arr
    }
    const bench = Math.floor(arr.length / 2)
    return merge(split(arr.splice(0, bench)), split(arr))
  }
  const merge = (left, right) => {
    const result = []
    // 不断比较左右数组首位
    while (left.length && right.length) {
      if (left[0] < right[0]) {
        result.push(left.shift())
      } else {
        result.push(right.shift())
      }
    }
    // 返回从小到大排好的数组并加上遗留数组元素
    return left.length ? result.concat(left) : result.concat(right)
  }
  return split([].concat(list))
}
console.info(mergeSort())

```