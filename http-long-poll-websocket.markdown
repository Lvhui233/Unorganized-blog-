---

ajax长轮询与websocket

---

html代码

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>long polling</title>
	<script  type="text/javascript" src="http://s1.hqbcdn.com/??lib/jquery/jquery-1.7.2.min.js"></script> 
</head>
<body>
	<div id="msg"></div>  
	<input id="btn" type="button" value="测试" />
	<script  type="text/javascript" >  
    $(function(){  
        $("#btn").bind("click",{btn:$("#btn")},function(evdata){  
            $.ajax({  
                type:"POST",  
                dataType:"json",  
                url:"data.php",  
                timeout:80000,     //ajax请求超时时间80秒  
                data:{time:"80"}, //40秒后无论结果服务器都返回数据  
                success:function(data,textStatus){  
                    //从服务器得到数据，显示数据并继续查询  
                    if(data.success=="1"){  
                        $("#msg").append("<br>[有数据]"+data.text);  
                        evdata.data.btn.click();  
                    }  
                    //未从服务器得到数据，继续查询  
                    if(data.success=="0"){  
                        $("#msg").append("<br>[无数据]");  
                        evdata.data.btn.click();  
                    }  
                },  
                //Ajax请求超时，继续查询  
                error:function(XMLHttpRequest,textStatus,errorThrown){  
                    if(textStatus=="timeout"){  
                        $("#msg").append("<br>[超时]");  
                        evdata.data.btn.click();  
                    }  
                }  
  
            });  
        });  
  
    });  
</script>  
</body>
</html>
```

php代码

若set_time_limit()设置超时时间,sleep的时间是不记在超时时间内的

```
<?php  
    if(empty($_POST['time']))exit();        
    set_time_limit(0);//无限请求超时时间        
    $i=0;        
    while (true){        
        //sleep(1);        
        usleep(500000);//0.5秒        
        $i++;        
                
        //若得到数据则马上返回数据给客服端，并结束本次请求        
        $rand=rand(1,999);        
        if($rand<=15){        
            $arr=array('success'=>"1",'name'=>'xiaoyu','text'=>$rand);        
            echo json_encode($arr);        
            exit();        
        }        
                
        //服务器($_POST['time']*0.5)秒后告诉客服端无数据        
        if($i==$_POST['time']){        
            $arr=array('success'=>"0",'name'=>'xiaoyu','text'=>$rand);        
            echo json_encode($arr);        
            exit();        
        }        
    }     
?>  
```