var GLOBAL=GLOBAL||{}
Ext.onReady(function(){
	$('.nav_content_wrap').css("height",$(window).height()-100+'px');
	var hideTimer=null;
	var moveTimer=null;
	$('.main_nav').delegate('li','mouseenter',function(){
		$this=$(this);
		clearInterval(moveTimer);
		moveTimer=setTimeout(function(){
			$('.main_nav .now').removeClass('now');
			$this.addClass('now');
			var nowindex=$this.index('.main_nav li');
			$('.nav_content_wrap').slideDown();
			$('.one_nav_content_wrap').eq(nowindex).css({'z-index':"1001"}).fadeIn(300).addClass('now');
			setTimeout(function(){
				$('.one_nav_content_wrap').each(function(i,ele){
					if(i!=nowindex){
						$(ele).slideUp(300).css({'z-index':'1000'}).removeClass('now')
					}
				})
			},300)
			
		},300)
	});
	$('.nav_wrap').mouseleave(function(){
		hide();
	});
	$(".nav_content").delegate('li','click',function(){
		hide();
	})
	$(".nav_moveout").mouseenter(function(){
		hide();
	});
	
	function hide(){
		$('.one_nav_content_wrap, .nav_content_wrap').fadeOut(300);
		clearInterval(moveTimer);
		$('.main_nav .now').removeClass('now');
	}
	
	$('.nav_content').delegate('li','click',function(){
		if($(this).attr('iLink')){
			var index=$(this).parent().index($('.one_nav_content'));
			
			var pathName=$('.main_nav li').eq(index).text()+'/'+$(this).text();
			loadPage($(this).attr('iLink'),pathName,$(this).attr("id"))
		}
	})
	
	
	//调整iframe的宽高
	var iframeResizeTimer = null;
	$(window).resize(function(){

		clearInterval(iframeResizeTimer);

		iframeResizeTimer = setTimeout(function(){

			//调整导航栏覆盖层高度
			$(".nav_content_wrap").css("height",$(window).height()-100+'px');

			//调整iframe高度
			$("#mainframe").height( $(window).height()-100);

		},200)

	});
	
	
	 GLOBAL.modifPwd = GLOBAL.EditWin || new Ext.custom.basicWindow({
                    title : '修改密码',
                    width : 480,
                    height : 300,
                    items : [new Ext.form.Panel({
                        width: '100%',
                        height:'100%',
                        bodyPadding: 10,
                        border:0,
                        items:[new Ext.custom.middletextfield({
                            margin:'10 0 0 30',
                            inputType : 'password',
	    					itemId : "userPwd",
                            fieldLabel : '当前密码',
                            beforeLabelTextTpl: required,
                            labelAlign : 'right'
                        }),new Ext.custom.middletextfield({
                            margin:'10 0 0 30',
                            inputType : 'password',
	    					itemId : "newUserPwd",
                            fieldLabel : '新密码',
                            beforeLabelTextTpl: required,
                            labelAlign : 'right'
                        }),new Ext.custom.middletextfield({
                            margin:'10 0 0 30',
                            beforeLabelTextTpl: required,
                            inputType : 'password',
	    					itemId : "newUserPwdAgain",
	    					fieldLabel : '确认密码',
                            labelAlign : 'right'
                        }),{
                            layout : 'hbox',
                            margin:'10 0 0 140',
                            arrowAlign: 'right',
                            items : [{
                                xtype:'button',
                                margin:'10 0 0 10',
                                width:80,
                                height:30,
                                handler : function(){
					//获取新密码和新密码的重复
					var newUserPwd = GLOBAL.modifPwd.down("#newUserPwd").getValue(),
					newUserPwdAgain = GLOBAL.modifPwd.down("#newUserPwdAgain").getValue();
						//判断俩次密码是否相同
				    if(newUserPwd != newUserPwdAgain){
				    	//ext 弹出框
						Ext.Msg.alert("温馨提示", "两次输入密码不一致！");
						return;
				    }
				    //把俩个值存到json中
				    var userPwds = {
				    	//原密码
						userPwd : GLOBAL.modifPwd.down("#userPwd").getValue(),
						//新密码
						newPwd : newUserPwd
				    };

				 	//修改密码,设置安全保护问题
					$.ajax({
				        async: false,
				        url: BPR.domain + "/Handler/AdminHandler.ashx?action=updatepass",
				        type: "POST",
				        data : userPwds,
				        dataType: "json"
				    }).done(function (data) {
				    	//判断是否登录   返回数据 data err login sueccss
				    	errTip(data, function(){
				    		Ext.Msg.alert("温馨提示", "修改密码成功");
				    		GLOBAL.modifPwd.down("#userPwd").setValue("");
				    		GLOBAL.modifPwd.down("#newUserPwd").setValue("");
				    		GLOBAL.modifPwd.down("#newUserPwdAgain").setValue("");
				    		GLOBAL.modifPwd.hide();
                    	});
				    }).fail(function () {
			
				    }).always(function () {
		
				    });
				},
                                style :'background:#6EC131;border:0',
                                text:'确定'
                            },{
                                xtype:'button',
                                margin:'10 0 0 30',
                                width:80,
                                height:30,
                                text:'取消',
                                handler : function(){
                                    GLOBAL.EditWin.hide();
                                }
                            }]
                        }]
                    })]
                });
        
	
	//加载中部内容(页面进入时，加载首页)
    loadHashPage();
	SecurityQuestion();
	
	
	
});





function loadPage(link,pathName,id){
	$('#backHomeContent').html('<iframe id="mainframe" src="'+link+'" width="100%" height="'+($(window).height()-100)+'" marginwidth="0" marginheight="0" hspace="0" vspace="0" frameborder="0" scrolling="auto"></iframe>');
	window.location.hash=id;
	$("#mainframe").load(function(){
		var nowLink="'backHomeIndex'";
		$(this).contents().find(".xn_nowpath").html('<a onclick="parent.loadPage('+nowLink+')">首页</a>/'+pathName+"");
		$(this).contents().find("body").css('overflow-x',"hidden");
	})
}
//刷新页面时加载相应内容页
function loadHashPage(){
	//判断哈希值以实现刷新页面时加载相应内容页
	if(window.location.hash){
		var menuHashId = window.location.hash.substring(1);
		if($("#" + menuHashId).length > 0){
			$("#" + menuHashId).trigger('click');
		}else{
			loadPage('backHomeIndex.html','','');
		}

	}else{
		loadPage('backHomeIndex.html','','');
	}
}
//读取用户基本数据
function SecurityQuestion(){

	$.ajax({
	    async: false,
	    url: BPR.domain + "/Handler/AdminHandler.ashx?action=returnuserinfo",
	    type: "POST",
	    dataType: "json",
	   contentType: "application/json"
	}).done(function (result) {
		errTip(result, function(){
			$("#userName").text(result.turename);
		});
	}).fail(function () {
	}).always(function () {

	});
}
//退出系统
function exitSystem(){
	$.ajax({
        async: false,
        url: BPR.domain + "/Handler/AdminHandler.ashx?action=quit",
        type: "GET",
        dataType: "json"
    }).done(function (result) {
    	errTip(result,function(){
			Ext.Msg.alert("提示",result.success,function(){
				window.location = "backLogin.html";
			});

    	});
    }).fail(function () {

    }).always(function () {
		
    });
}
function modifyPassword(){
	GLOBAL.modifPwd.show();
}