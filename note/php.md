## PHP

提取字符串中图片路径

```
function getImgUrl($content)
{
    $pattern = "/<[img|IMG].*?src=[\'|\"](.*?(?:[\.gif|\.jpg]|))[\'|\"].*?[\/]?>/"; //正则
    preg_match_all($pattern, $content, $match); //匹配图片
    return $match[1]; //返回所有图片的路径
}
```

日志记录

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

将（一维、二维）对象数组转化为数组

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

