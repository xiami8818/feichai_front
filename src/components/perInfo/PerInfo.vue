<template>
    <div id="pi">
        <div id="personInfo">
              <img :src="img">
              <h2>{{name}}</h2>
               <div id="word">
               <p id="word1">账号管理</p>
               </div>
            <ul id="list">
            <li id="list1" @click="first"><span><i class="iconfont">&#xe6b8;</i>个人信息</span></li>
            <li id="list2" @click="second"><span><i class="iconfont">&#xe6ae;</i>账号设置</span></li>
            </ul>
        </div>
      <div id="personInfo1" v-if="firstName=='true'">
       <p>基本信息</p>
          <label class="tip " id="nameTip">姓名：</label>
          <input type="text" class="text" id="nameText" name="username"  lay-verify="required" placeholder="请输入您的真实姓名" v-model="newTrueName" />
        <label class="tip" id="qqTip"> QQ：</label>
          <input type="text" id="qqText" class="text" name="username"  lay-verify="required" placeholder="请输入您的QQ号" v-model="newQQ" />
          <label class="tip" id="numTip">学号：</label>
          <input type="text" class="text" id="numText" name="username"  lay-verify="required" placeholder="请输入您的学号" v-model="newNum" />
        <label class="sexTip" id="sexTip">性别：</label>
        <label id="man"><input name="sex" type="radio" @click="man" id="manRadio" />男</label>
        <label id="woman"><input type="radio" name="sex" @click="woman" id="womanRadio" />女</label>
          <label class="tip" id="schoolTip">学校：</label>
          <input type="text" class="text" id="schoolText" lay-verify="required" placeholder="请输入您的学校" v-model="newSchool" /><span></span>
          <button id="save" :disabled="isChanged" @click="save">保存</button>
          <button id="recovery" @click="recovery" :disabled="isChanged1">恢复</button>
      </div>
        <div id="personInfo2" v-if="secondName=='true'">
            <p>账号设置</p>
            <label id="headerTip">头像</label>
            <img src="http://47.100.137.63:8080/cat.jpg" id="img">
            <span id="change" @click="changeBlock1">{{changeHeader}}</span>
            <div id="imgGroup" v-if="change1">
                <img src="http://47.100.137.63:8080/cat.jpg" id="img1">
                <img src="http://47.100.137.63:8080/boy.jpg" id="img2">
                <img src="http://47.100.137.63:8080/cow.jpg" id="img3">
                <img src="http://47.100.137.63:8080/girl.jpg" id="img4">
                <img src="http://47.100.137.63:8080/pikachu.jpg" id="img5">
                <img src="http://47.100.137.63:8080/ghost.jpg" id="img6">
            </div>
          <hr id="horizon">
            <label class="nick" id="nickName">昵称</label>
            <label class="nickText" id="nickWord">Magnetic达</label>
            <span id="modify" @click="changeBlock2">更改</span>
            <div id="nickNameGroup" v-if="change2">
              <label class="labelList" id="labelList1">请输入新的昵称</label>
                <input class="inputList" id="inputList1">
                <label class="labelList2" id="labelList13">不得超过10个字符</label>
                <button class="buttonList" id="buttonList1">确定</button>
                <button class="buttonList2" id="buttonList3" @click="off">取消</button>
            </div>
            <hr id="horizon1">
             <label class="password" id="password1">密码</label>
            <label class="passwordText" id="passwordWord">**********</label>
            <span id="changePassword" @click="changeBlock3">修改</span>
            <div id="passwordGroup" v-if="change3">
                <label class="labelList4" id="labelList5">请输入新的密码</label>
                <input class="inputList2" id="inputList3">
                <label class="labelList8" id="labelList19">不得超过10位</label>
                <label class="labelList6" id="labelList7">再次输入你的密码</label>
                <input class="inputList4" id="inputList5">
                <button class="buttonList4" id="buttonList5">确定</button>
                <button class="buttonList6" id="buttonList7" @click="off">取消</button>
            </div>
            <hr id="horizon2">
        </div>
      <Bottom id="bottom"></Bottom>
    </div>
</template>

<script>
import Bottom from "@/components/bottom/Bottom";
import axios from "axios";
    export default {
        name: "PerInfo",
      components: {
          Bottom
      },
      methods: {
            off(){
                this.change1=false;
                this.change2=false;
                this.change3=false;
            },
            changeBlock1(){
              this.change1=!this.change1;
                this.change2=false;
                this.change3= false;
                if(this.change1){
                  this.changeHeader = '保存';
                }else {
                  this.changeHeader = '更换';
                }
            },
          changeBlock2(){
              this.change1=false;
              this.change2=true;
              this.change3=false;
          },
          changeBlock3(){
              this.change1=false;
              this.change2=false;
              this.change3=true;
          },
        first(){
           this.firstName='true';
           this.secondName='false';
      },
        second(){
           this.firstName='false';
            this.secondName='true';
      },
          man(){
            this.newSex = '男';
          },
        woman(){
            this.newSex = '女';
        },
        save() {
            const that = this;
          axios.post("http://47.100.137.63:8080/user/setUser?trueName="+that.newTrueName+"&qq="+that.newQQ+"&num="+that.newNum+"&sex="+that.newSex+"&school="+that.newSchool).then(function (res){
            if(res.data == "$success"){
              console.log("true");
              that.$router.go(0);
            }else {
              alert("修改失败！");
            }
          }).catch(function (){
            alert("发生未知错误！");
          });
        },
        recovery(){
            this.newTrueName = this.trueName;
            this.newQQ = this.qq;
           this.newNum = this.num;
           this.newSex = this.sex;
           console.log(this.sex+" "+this.newSex);
           if(this.sex =='男'){
             document.getElementById('manRadio').checked = 'checked';
             document.getElementById('womanRadio').checked = false;
           }else if(this.sex =='女'){
             document.getElementById("manRadio").checked = false;
             document.getElementById('womanRadio').checked = 'checked';
           }else {
             document.getElementById("manRadio").checked = false;
             document.getElementById('womanRadio').checked = false;
           }
           this.newSchool = this.school;
        },
        getUsre(){
            const that = this;
            axios.get("http://47.100.137.63:8080/user/getUser").then(function (res){
              if(res.data == '$false'){
                that.$route.push('/index');
                return ;
              }
              let temp = res.data.split("$");
              that.trueName = temp[0];
              that.qq = temp[1];
              that.num = temp[2];
              that.sex = temp[3];
              if(temp[3] == '男'){
                document.getElementById("manRadio").checked = 'checked';
              }else if(temp[3] == '女'){
                document.getElementById("womanRadio").checked = 'checked';
              }
              that.school = temp[4];
              that.img = temp[5];
              that.name = temp[6];
              that.newTrueName = that.trueName;
              that.newQQ = that.qq;
              that.newNum = that.num;
              that.newSex = that.sex;
              that.newSchool = that.school;
            })
        }
      },
      data(){
          return {
            sex:'',
            name:'',
            trueName: '',
            newTrueName: '',
            qq:'',
            num:'',
            school: '',
            img: '/unLogin.jpg',
            newSex: '',
            newName:'',
            newQQ:'',
            newNum:'',
            newSchool:'',
            isChanged:'disabled',
            isChanged1: 'disabled',
              firstName:'true',
              secondName:'false',
              change1: false,
              change2: false,
              change3: false,
            changeHeader:'更换'
          }
      },
      mounted() {
        this.getUsre();
      },
      watch: {
        newTrueName: function () {
          if (this.newTrueName != this.trueName) {
            this.isChanged = false;
            this.isChanged1 = false;
          } else {
            this.isChanged1 = 'disabled';
            if(this.newQQ == this.qq && this.newNum ==this.num && this.newSex == this.sex && this.newSchool == this.school){
              this.isChanged = 'disabled';
            }
          }
          if(this.newTrueName == '' || this.newNum == '' || this.newQQ == '' || this.newSex == '' || this.newSchool == ''){
            this.isChanged = 'disabled';
          }
        },
        newQQ: function () {
          if (this.newQQ != this.qq) {
            this.isChanged = false;
            this.isChanged1 = false;
          } else {
            if(this.newTrueName == this.trueName && this.newNum ==this.num && this.newSex == this.sex && this.newSchool == this.school){
              this.isChanged = 'disabled';
            }
            this.isChanged1 = 'disabled';
          }
          if(this.newQQ == '' || this.newNum == '' || this.newTrueName == '' || this.newSex == '' || this.newSchool == ''){
            this.isChanged = 'disdabled';
          }
        },
        newNum: function () {
          if (this.newNum != this.num) {
            this.isChanged = false;
            this.isChanged1=false;
          } else {
            if(this.newQQ == this.qq && this.newTrueName ==this.trueName && this.newSex == this.sex && this.newSchool == this.school){
              this.isChanged = 'disabled';
            }
            this.isChanged1 = 'disabled';
          }
          if(this.newNum == '' || this.newTrueName == '' || this.newQQ == '' || this.newSex == '' || this.newSchool == ''){
            this.isChanged = 'disdabled';
          }
        },
        newSex: function () {
          if (this.newSex != this.sex) {
            this.isChanged = false;
            this.isChanged1 = false;
          } else {
            if(this.newQQ == this.qq && this.newNum ==this.num && this.newTrueName == this.trueName && this.newSchool == this.school){
              this.isChanged = 'disabled';
            }
            this.isChanged1 = 'disabled';
          }
          if(this.newTrueName == '' || this.newNum == '' || this.newQQ == '' || this.newSex == '' || this.newSchool == ''){
            this.isChanged = 'disdabled';
          }
        },
        newSchool: function () {
          if (this.newSchool != this.school) {
            this.isChanged = false;
            this.isChanged1 = false;
          } else {
            if(this.newQQ == this.qq && this.newNum ==this.num && this.newSex == this.sex && this.newTrueName == this.trueName){
              this.isChanged = 'disabled';
            }
            this.isChanged1 = 'disabled';
          }
          if(this.newTrueName == '' || this.newNum == '' || this.newQQ == '' || this.newSex == '' || this.newSchool == ''){
            this.isChanged = 'disdabled';
          }
        }
      }
    }
</script>

<style scoped>
@font-face {
    font-family: 'iconfont';
    src: url('../../assets/font/iconfont.eot');
    src: url('../../assets/font/iconfont.eot?#iefix') format('embedded-opentype'),
    url('../../assets/font/iconfont.woff') format('woff'),
    url('../../assets/font/iconfont.ttf') format('truetype'),
    url('../../assets/font/iconfont.svg#iconfont') format('svg');
}
.iconfont{
    color: darkgrey;
    margin-top: auto;
    font-family:"iconfont" !important;
    font-size:15px;
    font-style:normal;
    -webkit-font-smoothing: antialiased;
    -webkit-text-stroke-width: 0.2px;
    -moz-osx-font-smoothing: grayscale;
}
#pi{
    width: 100%;
    height: 100%;
}
#personInfo {
    position: absolute;
    margin-top: 7vh;
    margin-left: 5vw;
    width: 24vw;
    height: 60vh;
    border: 2px solid cadetblue;
}
#personInfo img{
    position: relative;
    width: 20%;
    border-radius: 100%;
    border: solid black 1px;
    left: 40%;
    top: 10px;
}
#personInfo h2 {
    position: relative;
    width: 100%;
    text-align: center;
}
#word1{
    -webkit-margin-before: 0;
    -webkit-margin-after: 0;
    margin-left: 1.5rem;
    font-size: 1.1rem;
    letter-spacing:0.1rem;
    padding-top: 0.5rem;
}
#bottom {
    position: absolute;
    top: 88vh;
}
#list{
    list-style: none;
    padding-left: 0;
}
#list1{
    border-left: 2px solid #4cacff;
    background: #e6f3ff;
    cursor: pointer;
    height: 6vh;
}
#list1 span{
    float: left;
    margin-left: 3rem;
    margin-top: 0.4rem;
}
#list1 i{
    margin-right: 0.5rem;
}
#list2{
    cursor: pointer;
    height: 6vh;
}
#list2 span{
    float: left;
    margin-left: 3rem;
    margin-top: 0.4rem;
}
#list2 i{
    margin-right: 0.5rem;
}
#personInfo1{
  position: absolute;
   left: 32vw;
  top: 18vh;
    width: 60vw;
    height: 63vh;
    background-color: #fff9fc;
}
#personInfo1 p{
    margin-left: 3vw;
    margin-top: 3vh;
    width: 60vw;
    font-family: 新宋体;
    float: left;
}
.tip{
    width: 10%;
    height: 5%;
    border-style: none;
}
.text {
    width: 46%;
    margin-bottom: 0.5rem;
    height: 5vh;
}
#nameTip {
    position: absolute;
    top: 16%;
    left: 14%;
    border-style: none;
}
#nameText {
    position: absolute;
    top: 15%;
    left: 22%;
}
#qqTip {
  position: absolute;
  top: 30%;
  left: 14%;
}
#qqText {
  position: absolute;
  top: 29%;
  left: 22%;
}
#numTip {
  position: absolute;
  top: 44%;
  left: 14%;
}
#numText {
  position: absolute;
  top: 43%;
  left: 22%;
}
#schoolTip {
  position: absolute;
  top: 72%;
  left: 14%;
}
#schoolText {
  position: absolute;
  top: 71%;
  left: 22%;
}
#sexTip {
  position: absolute;
  top: 58%;
  left: 14%;
}
#man {
  position: absolute;
  top: 57%;
  left: 22%;
  font-size: 1.4rem;
}
#man input {
  position: relative;
  width: 20px;
  height: 20px;
}
#woman {
  position: absolute;
  top: 57%;
  left: 30%;
  font-size: 1.4rem;
}
#woman input {
  position: relative;
  width: 20px;
  height: 20px;
}
#recovery {
  position: absolute;
  top: 6%;
  left: 80%;
  width: 8%;
  height: 8%;
  background-color: #e6f3ff;
  border-radius: 1rem;
  border: 0;
  outline-style: none;
}
#save {
  position: absolute;
  top: 6%;
  left: 90%;
  width: 8%;
  height: 8%;
  background-color: #e6f3ff;
  border-radius: 1rem;
  outline-style: none;
  border: 0;
}
#personInfo2{
    position: absolute;
    left: 32vw;
    top: 18vh;
    width: 60vw;
    height: 68vh;
    background-color: #fff9fc;
}
#personInfo2 p{
    margin-left: 3vw;
    margin-top: 3vh;
    width: 60vw;
    font-family: 新宋体;
}
#headerTip{
   position: relative;
  margin-left: 10%;
  height: 20%;
  top: -4vh;
  font-size: 1.2rem;
}
#img{
    margin-left: 26%;
    width: 10%;
    border-radius: 3rem;
    position: relative;
}
#imgGroup img {
  position: relative;
  border-radius: 100%;
  height: 100%;
  margin-left: 2%;
  z-index: 2;
}
#change{
    height: 5%;
    left:75%;
    top: 23%;
    cursor: pointer;
    color: #4cacff;
    position: absolute;
}
#horizon{
    width: 80%;
    position: relative;
    margin-bottom: 2%;
}
#imgGroup{
    position: relative;
    height: 20%;
    width: 80%;
  margin-left: 14%;
}
.nick{
    height: 5%;
    width: 10%;
}
#nickName{
    position: relative;
  font-size: 1.2rem;
  margin-left: 10%;
}
.nickText{
    height: 5%;
    width: 40%;
}
#nickWord{
    position: relative;
  margin-left: 27%;
    top: 10%;
}
#modify{
    height: 5%;
    top: 10%;
    margin-left:22%;
    cursor: pointer;
    color: #4cacff;
    position: relative;
}
#horizon1{
    width: 80%;
    position: relative;
    margin-bottom: 2%;
}
#nickNameGroup{
    position: relative;
    height: 30%;
    width: 80%;
    margin-left: 14%;
}
.labelList{
    height: 5%;
    width: 10%;
}
#labelList1{
    position: relative;
    margin-left: 5%;
    top: 15%;
}
.inputList{
   height: 15%;
    width: 20%;
}
#inputList1{
   position: relative;
    top: 15%;
    margin-left: 10%;
}
.labelList2{
   height: 5%;
    width: 10%;
    color: darkgrey;
}
#labelList13{
  position: relative;
    margin-left: 5%;
    top: 15%;
}
.buttonList{
    width: 10%;
    height: 25%;
    background-color: #4cacff;
    border-radius: 3rem;
    border-style: none;
    outline-style: none;
}
#buttonList1{
  top: 30%;
    position: relative;
    margin-left: 20%;
}
.buttonList2{
    width: 10%;
    height: 25%;
    border-radius: 3rem;
    border-style: none;
    outline-style: none;
}
#buttonList3{
    top: 30%;
    position: relative;
    margin-left: 10%;
}
.password{
  height: 5%;
    width: 10%;
    font-size: 1.2rem;
}
#password1{
   position: relative;
    margin-left: 10%;
}
.passwordText{
    height: 5%;
    width: 20%;
}
#passwordWord{
    position: relative;
    margin-left: 28%;
}
#changePassword{
    height: 5%;
    margin-left:23%;
    cursor: pointer;
    color: #4cacff;
    position: relative;
}
#passwordGroup{
    position: relative;
    height: 40%;
    width: 80%;
    margin-left: 14%;
}
#horizon2{
    width: 80%;
    position: relative;
    margin-bottom: 2%;
}
.labelList4{
    height: 5%;
    width: 20%;
}
#labelList5{
    position: relative;
    margin-left: 5%;
    margin-bottom: 2%;
    top: 15%;
}
.inputList2{
    height: 15%;
    width: 35%;
}
#inputList3{
    position: relative;
    top: 15%;
    margin-left: 5%;
    margin-bottom: 2%;
}
.labelList8{
    height: 5%;
    width: 20%;
    color: darkgrey;
}
#labelList19{
    position: relative;
    margin-left: 5%;
    margin-right: 10%;
    margin-bottom: 2%;
    top: 15%;
}
.labelList6{
    height: 5%;
    width: 10%;
}
#labelList7{
    position: relative;
    margin-left: 5%;
    top: 15%;
}
.inputList4{
    height: 15%;
    width: 40%;
}
#inputList5{
    position: relative;
    top: 15%;
    margin-left: 2.5%;
    margin-right: 10%;
}
.buttonList4{
    width: 10%;
    height: 25%;
    background-color: #4cacff;
    border-radius: 3rem;
    border-style: none;
    outline-style: none;
}
#buttonList5{
    top: 30%;
    position: relative;
    margin-left: 20%;
}
.buttonList6{
    width: 10%;
    height: 25%;
    border-radius: 3rem;
    border-style: none;
    outline-style: none;
}
#buttonList7{
    top: 30%;
    position: relative;
    margin-left: 10%;
}
</style>