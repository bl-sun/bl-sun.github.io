---
title: RGB与十六进制颜色转换
date: 2024-05-04 11:49:16
description:
top_img:
---

十六进制颜色值：<input id="v1" type="text" value="#34538b" /><input id="b1" type="button" value="转为RGB格式" />
<p id="showRgb"></p>
GRB颜色值：<input id="v2" type="text" value="RGB(23, 245, 56)" /><input id="b2" type="button" value="转为十六进制形式" />
<p id="showHex"></p>

<script type="text/javascript" js-pjax>
// by zhangxinxu welcome to visit my personal website http://www.zhangxinxu.com/
// 2010-03-12 v1.0.0
//十六进制颜色值域RGB格式颜色值之间的相互转换

//-------------------------------------
/*RGB颜色转换为16进制*/
String.prototype.colorHex = function(){
    var that = this;
    //十六进制颜色值的正则表达式
    var reg = /^#([0-9a-fA-f]{3}|[0-9a-fA-f]{6})$/;
    // 如果是rgb颜色表示
    if (/^(rgb|RGB)/.test(that)) {
        var aColor = that.replace(/(?:\(|\)|rgb|RGB)*/g, "").split(",");
        var strHex = "#";
        for (var i=0; i<aColor.length; i++) {
            var hex = Number(aColor[i]).toString(16);
            if (hex === "0") {
                hex += hex;
            }
            strHex += hex;
        }
        if (strHex.length !== 7) {
            strHex = that;
        }
        return strHex;
    } else if (reg.test(that)) {
    var aNum = that.replace(/#/,"").split("");
        if (aNum.length === 6) {
            return that;
        } else if(aNum.length === 3) {
            var numHex = "#";
            for (var i=0; i<aNum.length; i+=1) {
            numHex += (aNum[i] + aNum[i]);
            }
            return numHex;
        }
    }
    return that;
};

//-------------------------------------------------

/*16进制颜色转为RGB格式*/
String.prototype.colorRgb = function(){
    var sColor = this.toLowerCase();
    //十六进制颜色值的正则表达式
    var reg = /^#([0-9a-fA-f]{3}|[0-9a-fA-f]{6})$/;
    // 如果是16进制颜色
    if (sColor && reg.test(sColor)) {
        if (sColor.length === 4) {
            var sColorNew = "#";
            for (var i=1; i<4; i+=1) {
            sColorNew += sColor.slice(i, i+1).concat(sColor.slice(i, i+1));
            }
            sColor = sColorNew;
        }
        //处理六位的颜色值
        var sColorChange = [];
        for (var i=1; i<7; i+=2) {
            sColorChange.push(parseInt("0x"+sColor.slice(i, i+2)));
        }
        return "RGB(" + sColorChange.join(",") + ")";
    }
    return sColor;
};
</script>
<script type="text/javascript" js-pjax>
var obj = {
    v1: document.getElementById("v1"),
    b1: document.getElementById("b1"),
    s1: document.getElementById("showRgb"),
    v2: document.getElementById("v2"),
    b2: document.getElementById("b2"),
    s2: document.getElementById("showHex")
};
obj.b1.onclick = function(){
    var v = obj.v1.value;
    obj.s1.innerHTML = v.colorRgb();
};
obj.b2.onclick = function(){
    var v = obj.v2.value;
    obj.s2.innerHTML = v.colorHex();
};
</script>
