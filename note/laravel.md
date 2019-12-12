## Laravel



##### 查询转数组

```
# $sql->map(function ($value) {return (array)$value;})->toArray();

注：$sql 对应查询语句
```

##### 表单验证

```
$captcha = Validator::make($this->param, [
            'content' => 'required',
        ], [
            'content.required' => '反馈内容必须！',
        ]);

if ($captcha->fails()) {
    $failMsg = $captcha->getMessageBag()->toArray();
    return ['msg'=>current($failMsg)[0]];
}
```

