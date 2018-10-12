<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
<title>Untitled Document</title>
<script>
 $('.title').on('click',function(){
            var id=$(this).data('id');
            alert(id);
    $('.modal-body').html('loading');
       $.ajax({
        type: 'POST',
        url: 'http://localhost/news.php?action=getdetailsbyid',
        data:{id: id},
        success: function(data) {
          $('.modal-body').html(data);
        },
        error:function(err){
          alert("error"+JSON.stringify(err));
        }
    })
 });
</script>
<style>
body{background:url(news-bg.jpg) repeat center center;background-size:contain;}
header{background:#222;margin-bottom:15px;border-bottom:5px solid #be2819}
.container-fluid .news-wrapper{margin-bottom:15px}
h1{color:#fff;margin:0 auto;padding:15px 0;font-size:35px;text-align:center;text-transform:capitalize}
.news-wrapper .item{min-height:100px;border-radius:10px;}
.news-wrapper .item a{display:block;margin-top:30px}
.news-wrapper h4{color:#222;margin:0 auto;font-size:17px;font-weight:600}
.news-wrapper .icon{float:left;margin:2px 6px 0 0;border:6px solid #be2819}
.news-wrapper .modal-dialog{margin-top:150px}
.news-wrapper button.close{padding: 2px 7px 3px;
cursor: pointer;
background: #be2819;
border: 0;
opacity: 1;
color: #fff;
border-radius: 41px;
margin: 1px 1px;}
.news-wrapper .close:focus,.news-wrapper .close:hover{background:#000;color:#fff;opacity:0.8}
.news-wrapper .modal-open{background:#00000080;padding-right:0}
</style>
</head>

<body>
<header><h1>India technology news</h1></header>
    <div class="container-fluid news-wrapper">
        <div class="row">
            <div class="col-sm-12">
            <?php
               $url = file_get_contents('https://newsapi.org/v2/top-headlines?country=in&category=technology&apiKey=bd6eac94f38747efb12dcd2a4a5684f9');
               $data = json_decode($url, true);
               $items = $data['articles'];
                   for($i = 0; $i < count($items); $i++) 
                    {
                         $news = $data['articles'][$i];
                         $news = ['index' => $i] + $news;
                         ?>
                   <div class="item col-sm-4">
                       <a class="title" data-toggle="modal" data-id="<?php echo $news['index']; ?>" href="#myModal<?php echo $news['index']; ?>">
                       		<div class="icon"></div>
                             <h4><?php echo $news['title'];?></h4>
                       </a>
                   </div>
                  
                   <div class="modal" id="myModal<?php echo $news['index']; ?>">
                       <div class="modal-dialog">
                           <div class="modal-content">
                               <div class="content">
                               	 <div class="modal-header">
                                   <button type="button" class="close" data-dismiss="modal">&times;</button>
                                   <?php echo "<h4>".$news['title']."</h4>"; ?>
                                   </div>
                                   <div class="modal-body">
                                   <?php 
                                         
										 if($news['description']== null)
                                         {
										 echo "<strong>"."Description: "."</strong>"."Not Available"."</br>";
										 }
										 else
										 {
                                         echo "<strong>"."Description: "."</strong>".$news['description']."</br>";
										 } 
										 if($news['publishedAt']== null)
										 {
										 echo "<strong>"."PublishedAt: "."</strong>"."Not Available"."</br>";
										 }
										 else
										 {
                                         echo "<strong>"."PublishedAt: "."</strong>".$news['publishedAt']."</br>";
										 }
										  if($news['author']== null)
										 {
										 echo "<strong>"."Author: "."</strong>"."Not Available"."</br>";
										 }
										 else
										 {
                                         echo "<strong>"."Author: "."</strong>".$news['author'];
										 }
                                         if($news['urlToImage']== null)
                                         {
                                         echo '<img class="img-responsive" src="https://via.placeholder.com/805x456">';
                                         }
                                         else
                                         {
                                         echo '<img class="img-responsive" src="'.$news['urlToImage'].'">';
                                    }?>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                   <?php } ?>
            </div>
        </div>
    </div>
</body>
</html>

