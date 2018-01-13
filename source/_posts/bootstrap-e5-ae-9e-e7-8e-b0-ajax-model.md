---
title: Bootstrap 实现 ajax model
tags:
  - ajax
  - bootstrap
id: 1758
categories:
  - JQuery
date: 2014-04-27 17:16:46
---

默认的 Bootstrap 只能用当前页面的数据，没有ajax的功能，可以通过一点变通就可以实现。

`<button class="btn btn-primary" id="showDialog" >Add New Post Type</button>
` 

<!--more-->

`<script>
    $(function(){
        $('#showDialog').click(function(){
            $('.modal-body').load('../posttype/edit/',function(result){
                $('#myModal').modal({
                    keyboard: false,
                    backdrop:false
                });
            });
        });
    });
</script>`

`<div class="modal fade" id="myModal" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>

#### Add New Post Type

            </div>
            <div class="modal-body">
                Loading...
            </div>
        </div>
        <!-- /.modal-content -->
    </div>
    <!-- /.modal-dialog -->
</div>`