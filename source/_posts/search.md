---
title: 常见查找算法
date: 2017-07-16 23:30:07
updated: 2017-07-16 23:30:07
tags: 算法
categories: 数据结构与算法
---
查找定义：根据给定的某个值，在查找表中确定一个七关键字等于给定值得数据元素。

查找算法分类：

1. 静态查找和动态查找
2. 有序查找和无序查找

平均查找长度

　　查找算法分类：
　　
　　1）静态查找和动态查找；
　　　　注：静态或者动态都是针对查找表而言的。动态表指查找表中有删除和插入操作的表。
　　2）无序查找和有序查找。
　　　　无序查找：被查找数列有序无序均可；
　　　　有序查找：被查找数列必须为有序数列。
　　平均查找长度（Average Search Length，ASL）：需和指定key进行比较的关键字的个数的期望值，称为查找算法在查找成功时的平均查找长度。
　　对于含有n个数据元素的查找表，查找成功的平均查找长度为：ASL = Pi*Ci的和。
　　Pi：查找表中第i个数据元素的概率。
　　Ci：找到第i个数据元素时已经比较过的次数。



一、顺序查找
思路：一个一个的查
复杂度：o(n)
代码：
{% codeblock %}
function seqSearch($arr,$k){
    for($i=0;$i<count($arr);$i++){
        if($arr[$i] == $k){
            return $i;
        }
    }
    return false;
}

$items=[1,3,5,7,9,11,13,15];

var_dump(seqSearch($items, 4));
var_dump(seqSearch($items, 5));

示例2：
// $array为数组，$k为要查找的值
 function search2 ($array, $val ){
      $array[]=$val;
        $n = count($array);
        $i = 0;
        while( $array[$i] != $val ){ 
            $i++;
        }
       return ($i == $n-1) ? false : $i;
}
$items=[32,55,6,33,921,543,123,666];
var_dump(search2($items, 4));
var_dump(search2($items, 55));
{% endcodeblock %}

二、二分查找
思路：也称为是折半查找，属于有序查找算法。需要设定三个值，low=0；high=count-1;mid=(low+high)/2,用给定值k先与mid的关键字比较，若相等则查找成功；若mid值比k值大，则去查找前半部分，high=mid-1;若mid值比K值小，则去查找后半部分，low=mid+1;循环比较，直到mid==k,循环条件：low<=high
说明：必须是有序的，如果不是有序的可以先排序一下
复杂度：最坏情况下，关键词比较次数为log2(n+1)，且期望时间复杂度为O(log2n)；
代码：
{% codeblock %}
function binarySearch($arr,$k){
    $len = count($arr);
    $low = 0;
    $high = $len-1;
    while($low <= $high) {
        $mid = intval(($low + $high) / 2);
        if ($arr[$mid] == $k) {
            return $mid;
        } elseif ($arr[$mid] > $k) {
            $high = $mid-1;
        } else {
            $low = $mid+1;
        }
    }
    return false;

}

$array  = array(2,3,4,7,9,10,11,18,19,20,33);
var_dump(binarySearch($array,7));
{% endcodeblock %}

三、插值查找
思路：基于二分查找算法，将查找点的选择改进为自适应选择，可以提高查找效率。当然，差值查找也属于有序查找。
mid=low+(key-a[low])/(a[high]-a[low])*(high-low)
说明：对于表长较大，而关键字分布又比较均匀的查找表来说，插值查找算法的平均性能比折半查找要好的多。反之，数组中如果分布非常不均匀，那么插值查找未必是很合适的选择。　　
复杂度：查找成功或者失败的时间复杂度均为O(log2(log2n))。
代码：
{% codeblock %}
function binarySearch ($array, $val ){
    $low=0;
    $high=count($array)-1;
    while( $low<=$high ){
        if($low==$high){
            $mid=$low;
        }else{
            $mid= $low +( ( $val -$array[$low] )/( $array[$high]-$array[$low] ) ) * (  $high - $low  ) ;
            if($mid<0)return false;
        }
        if( $val<$array[$mid] ){
            $high=$high-1;
        }else if( $val>$array[$mid] ){
            $low=$low+1;
        }else{
            return $mid;
        }
    }
    return false;
}

$items=[1,3,5,7,9,11,13,15];

var_dump(binarySearch($items, 4));
var_dump(binarySearch($items, 5));
{% endcodeblock %}