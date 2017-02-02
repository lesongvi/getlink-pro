var $globalCode = '';
var $itval = null;

function redirect(){
	if($globalCode !== ''){
		clearInterval($itval);
		$itval = null;
		$("#main").submit();
	}
}
//1111111111111111111111
$(document).ready(function($) {
	$('input').on('keypress', function (e) {
        if(e.which === 13){
			$("#request").click();
        }
   });
});

function checkvalid(){
	$(".inputtxt").each(function(index, el) {
		$(this).attr({
			disabled: ''
		});
	});;
	$("#request").addClass('disabled');

	displayLoading();
	var link = $("#link").val();
	var pass='';

	if($("#password").val().trim() !== ''){
		var pass = $("#password").val();
	}
	var pattern = /fshare\.vn\/(?:file|folder)\/([a-zA-Z0-9]+)/i;
	var res = link.trim().match(pattern);

	if ($(res).length < 2) {
		displayAlert("Đường dẫn sai quy định !");
		$(".inputtxt").each(function(index, el) {
			$(this).removeAttr('disabled');
		});
		
		$("#request").removeClass('disabled');
		return;
	}

	var code = res[1];

	$.ajax({
		url: 'ajax/ajax.php',
		type: 'POST',
		dataType: 'json',
		data: {code: code, pass: pass}
	})
	.done(function(data) {
		if(data.error){
			displayAlert(getMessageString(data.error));
		}else if(data.success){
			// $("#result").html('');
			// var input = $("<input>")
   //             .attr("id", "url")
   //             .attr("type", "hidden")
   //             .attr("name", "linkcode").val($.trim(data.success));
			// $('#redirect').append($(input));
			// $("#main").submit();
			$globalCode = $.trim(data.linkcodeEncrypt);
			$("#tokenCode").val($globalCode);
			$("#result").html('');
			$("#redirect").html('');
			$("#thongbao").modal('show',function(){
			    $("#chuyen").focus();
			});
			setTimeout(function(){
			    $("#chuyen").focus();
			}, 500);
			//testttttt
		}else if(data.folder){
			$("#folderName").html(data.name);
			var $append = '';
			var $class;
			$.each(data.folder,function(index, el) {
				$link = data.folder[index].linkcode;
				if(pass !== '') $link+='&pass='+pass;
				$class = '<a target="_blank" href="?link='+$link+'" class="list-group-item">'+data.folder[index].name+'</a>';
				$append += $class;
			});
			$("#result").html('');
			$("#modal-list").html($append);
			$("#myModal").modal('show');
		}
	})
	.fail(function(e) {
	})
	.always(function(e) {
		$(".inputtxt").each(function(index, el) {
			$(this).removeAttr('disabled');
		});

		$("#request").removeClass('disabled');
	});
}

function displayLoading(){
	$("#result").html('<img src="https://getlink.vaphim.com/images/01-progress.gif">');
}

function getMessageString($str){
	switch($str){
		case 'empty': return 'Không tìm thấy file.';
		case 'thief': return 'Who are you ?';
		case 'public': return 'File vi phạm bản quyền.';
		case 'deleted': return 'File đã bị xóa.';
		case 'folder': return 'Đây là link folder, bạn phải nhập link file.';
		case 'pass': return 'Sai mật khẩu !';
		case 'systemfile': return 'Không tìm thấy file';
	}
}

function displayAlert(message){
	var str = 
	'<div id="alert" class="alert alert-danger fade in">'+
     '<a href="#" class="close" data-dismiss="alert" aria-label="close">&times;</a>'+
     '<strong>Lỗi!</strong> '+message+
   	'</div>';
	$("#result").html(str);
}
