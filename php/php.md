## PHP

##### 二维数组排序

```
# $data 数组, field_name 字段名
array_multisort(
    array_column($data, 'field_name'),
    SORT_DESC,
    $data
);
```



##### 转字母大小写

```
strtoupper();//转大写
strtolower();//转小写
usfilst();//字符串首字母大写
ucwords();//单词首字母大写
```



##### 提取字符串中图片路径

```
function getImgUrl($content)
{
    $pattern = "/<[img|IMG].*?src=[\'|\"](.*?(?:[\.gif|\.jpg]|))[\'|\"].*?[\/]?>/"; //正则
    preg_match_all($pattern, $content, $match); //匹配图片
    return $match[1]; //返回所有图片的路径
}
```

##### 日志记录

```
$data = '*';//文本

$url = './storage/'.date('Ymd').'_update_bl.txt';
$dir_name=dirname($url);
//目录不存在就创建
if(!file_exists($dir_name))
{
//iconv防止中文名乱码
$res = mkdir(iconv("UTF-8", "GBK", $dir_name),0777,true);
}
$fp = fopen($url,"a");//打开文件资源通道 不存在则自动创建
fwrite($fp,date("Y-m-d H:i:s").var_export($data,true)."\r\n");//写入文件
fclose($fp);//关闭资源通道
```

##### 将（一维、二维）对象数组转化为数组

```
/**
     * 将（一维、二维）对象数组转化为数组
     * @param $data array 对象数组
     * @return mixed
     */
    public function alterArray($data){
        if (is_object($data)) {
            $array = (array)$data;

            if (!is_array($array)) {
                $array = $data->toArray();
            }
        } else {
            $array = $data;
        }

        $data = array_reduce($array, function ($value, $name) {
            return array_merge($value ?: [], [(array)$name]);
        });

        return $data;
    }
```



##### 解析获取某月第几周星期一对应的时间戳

```
/**
     * 解析获取某月第几周星期一对应的时间戳
     * @param $dataTime 3月5周
     * @return false|int
     */
    public function analysisWeek($dataTime)
    {
        $data = explode('月', $dataTime);
        $mouth = $data[0];//月份
        $data1 = explode('周', $data[1]);
        $week = $data1[0];//第几周

        $year = date('Y');
        if (date('m') == 12 && $mouth == 1) {
            $year = $year + 1;
        }

        $w = date('w', strtotime(date($year.'-'.$mouth.'-01')));//当月第一天为星期几
        $day = (int)7*$week - $w - 5;//当周星期对应第几号

        $day = $day > 0 ? $day : -$day;

        return strtotime(date($year.'-'.$mouth.'-'.$day));//推送日期时间戳
    }
```

