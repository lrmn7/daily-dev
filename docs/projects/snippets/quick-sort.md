**Last updated**: October 18, 2019

```js
const arr = [0, 2, 4, 5, 1, 3, 3, 7, 6, 9 ,2]
// arr.sort()
const swap = (items, leftIndex, rightIndex)=>{
    let temp = items[leftIndex];
    items[leftIndex] = items[rightIndex];
    items[rightIndex] = temp;
}
const sort = (arr)=>{
  quicksort(arr, 0, arr.length - 1)
}
const quicksort = (arr, left, right)=>{
  if (left >= right){
    return;
  }
  let pivot = arr[Math.floor((left + right) / 2)]
  let next = swapper(arr, left, right, pivot)
  quicksort(arr, left, next - 1)
  quicksort(arr, next , right)
}
const swapper = (arr, left, right, pivot) =>{
  while(left <= right){
    while(arr[left] < pivot){
      left++;
    }
    while(arr[right] > pivot){
      right--;
    }
    if(right >= left){
      swap(arr,left,right)
      left++;
      right--;
    }
  }
  return left
}

sort(arr)
```