<template>
   <div class="card">
       <div class="card-top card-com laca">
           <div class="siz">
              <img src="../assets/img/buuk (1).png" alt="">
           </div>
          <div class="card-size">
              知学习
          </div>
       </div>
        <div v-for="el in bookss" :key="el.pkid">
            <div class="card-b">
               <div class="card-book">
                 《{{el.title}}》共朗读<span>{{el.readTotal+el.readCount}}</span>  遍
               </div>
            </div>
            <div class="card-com">
               <div class="l-pu">
                    <div class="label label-f">今日朗读遍数</div>
                    <span></span>
                    <van-stepper @change="bookChang(el)" v-model.number="el.readCount" class="l-mi" :min="0" :disabled='first'  />
               </div>
            </div>
        </div>


        <div  v-for="(item,index) in books" :key="index">
           <div class="card-b card-book add-book">
             <span>书本 ({{index + booklength + 1}})</span>
             <span class="del-book" @click='cardDel(index)'  v-if="!first" >删除</span>
            </div>
           <div class="pdd">
              <div class="l-put put-bot">
               <div class="label label-f">书本名称</div> <input type="text" placeholder="请输入书本名称" v-model='item.bookName' :disabled='first' >
              </div>
              <div class="l-put">
               <div class="label label-f">朗读章节</div> <input type="text" placeholder="请输入" v-model='item.section' :disabled='first' >
              </div>
              <!-- <div class="l-put">
               <div class="label label-f">链接</div> <input type="text" placeholder="请输入链接" @blur="repLink(index)" v-model='item.link' :disabled='first' >
              </div> -->
           </div>
        </div>
        <div class="card-com card-add" v-if="!first">
            <span class="laca" @click='cardBook' >
               增加书本
              <div class="laca-add"><img src="../assets/img/add (1).png" alt=""></div>
            </span>
        </div>
        <div class="card-com card-share">
            经典名句分享
        </div>
        <div class="card-put">
          <textarea  rows="4" placeholder='请输入您要分享的经典名句'  v-model="pushData.classic" class="ccc" @blur='bblur'  :disabled='first' ></textarea>
        </div>

        <div class="card-top card-com laca card-ma">
           <div class="siz">
              <img src="../assets/img/chess (2).png" alt="">
           </div>
          <div class="card-size">
              行实践
          </div>
       </div>
       <div class="card-com">
          <div class="l-put">
               <div class="label label-f">修身</div> <input type="text" placeholder="请输入今日您对个人的贡献" v-model="pushData.practice.character" @blur='bblur' :disabled='first' >
          </div>
       </div>
       <div class="card-com">
          <div class="l-put">
               <div class="label label-f label-t">齐家</div> <input type="text" placeholder="请输入今日您对家庭和家人的贡献"  v-model="pushData.practice.family" @blur='bblur' :disabled='first' >
          </div>
       </div>
       <div class="card-com">
          <div class="l-put">
               <div class="label label-f label-t">建功</div> <input type="text" placeholder="请输入今日您对工作的贡献" v-model="pushData.practice.work" @blur='bblur' :disabled='first' >
          </div>
       </div>

        <div class="card-top card-com laca card-ma">
           <div class="siz">
              <img src="../assets/img/gift (2).png" alt="">
           </div>
          <div class="card-size">
              积善行
          </div>
       </div>
       <div class="card-b">
          <div class="card-book">
            发愿从{{pushCount.bonaStart}}起，{{pushCount.years}}年内{{pushCount.days * pushCount.bonaCount }}善事，累计<span>{{pushCount.bonaTotal+pushCount.bonaDays}}</span>善。
          </div>
       </div>
        <div class="card-com">
               <div class="l-pu">
                    <div class="label label-f">今日行善次数</div>
                    <van-stepper  v-model="pushCount.bonaDays" class="l-mi" :min="0" :disabled='first' />
               </div>
       </div>
       <div class="card-top card-com laca card-ma">
            <div class="tit-box">
              <div class="siz">
                  <img src="../assets/img/star (2).png" alt="">
              </div>
              <div class="card-size">
                  立志愿
              </div>
            </div>
       </div>
       <div class="card-put">
          <textarea  rows="4" placeholder='请输入您要分享的志愿'  v-model="pushData.volunteer" class="ccc" @blur='bblur' :disabled='first' ></textarea>
        </div>
        <div class="card-top card-com laca card-ma">
          <div class="tit-box">
            <div class="siz">
                <img src="../assets/img/awake.png" alt="">
            </div>
            <div class="card-size">
                省觉悟
            </div>
          </div>
       </div>
       <div class="card-put">
          <textarea  :disabled='first' rows="4" placeholder='请输入您的反省及觉悟'  v-model="pushData.introspective" class="ccc" @blur='bblur'  ></textarea>
        </div>
         <div class="card-top card-com laca card-ma">
           <div class="tit-box">
             <div class="siz">
                  <img src="../assets/img/hua.png" alt="">
              </div>
              <div class="card-size">
                  感谢
              </div>
           </div>
           
       </div>
       <div class="card-put">
          <textarea  :disabled='first' rows="4" placeholder='请输入您的感谢'  v-model="pushData.thanks" class="ccc tanks" @blur='bblur'  ></textarea>
        </div>

        <div class="card-btn" @click="maskFn">
           <div class="card-div">
              {{btnTitle}}
           </div>
        </div>
        <v-popup :popupShow="mask" :popupType="'tip1'" :tip-text="tipTxt" @sure="punch"></v-popup>
        <v-toast :mask='mask1' :toastTxt="'该链接格式错误'"></v-toast>
   </div>
</template>
<script>
import { recordPer , pushCard, groups } from '@/api/index'
export default {
  name:'card',
  data () {
    return {
      // booksss:[],
      num:'',
      books:[],
      bookss:[],
      pushCount: {},  //积善行
      pushData:{
        volunteer: '', // 立志愿
        classic: '',  // 经典名句
        practice: { character: '' ,work: '', family: ''},  //行～实践
        introspective: '', //省省悟
        thanks: '', //感谢
      },
      btnTitle:'提交',
      first:false,
      mask:false,
      mask1:false,
      tipTxt:'',
      booklength:0,
      repetition: false
    }
  },
  watch:{
    pushData:{
      deep:true,
      handler:function (newVal,oldVal){
        // if(newVal.bonaCount!=undefined&&newVal.pushCount.bonaStart!=undefined&&newVal.pushCount.bonaTotal!=undefined&&newVal.pushCount.days!=undefined&&newVal.pushCount.years!=undefined){
            localStorage.setItem('cardPushData',JSON.stringify(newVal));
        // }
      }
    }
  },
  methods: {
    repLink(i){
      let re=new RegExp('((http[s]{0,1}|ftp)://[a-zA-Z0-9\.\-]+\.([a-zA-Z]{2,4})(:\d+)?(/[a-zA-Z0-9\.\-~!@#$%^&*+?:_/=<>]*)?)|(www.[a-zA-Z0-9\.\-]+\.([a-zA-Z]{2,4})(:\d+)?(/[a-zA-Z0-9\.\-~!@#$%^&*+?:_/=<>]*)?)');
      let that=this;
      if(!re.test(this.books[i].link)){
        that.mask1=true;
        this.books[i].link=''
        return setTimeout(() => {
            that.mask1 = false;
        }, 1500);
      }
    },
    bookChang(el){
      if(el.readCount==0){
        el.type=1;
      }
    },
    maskFn(){
      if(this.btnTitle=='提交'){
        this.mask=true;
        this.tipTxt='确认提交打卡信息？'
      }else{
        this.mask=true;
        this.tipTxt='修改会覆盖今日打卡内容，是否继续？'
      }
    },
    gainPer() {
      let that=this;
       recordPer({}).then( res => {
          if(res.code == 1 || res.code == 402 ) {


              if(res.code == 402 ) {
                 this.btnTitle = '修改'
              } else {
                 this.btnTitle = '提交'
              }

              res.data.books.forEach((el,i) => {
                  el.num=0
                  if(res.code == 402){
                    el.num=el.readCount;
                  }
                  el.readTotal=el.readTotal-el.num;
                  if(!el.readCount == 0 ||el.type==1) {
                    this.bookss.push(el)
                  }
              })
              that.pushData.thanks = res.data.thanks
              that.pushData.practice = res.data.practice ? res.data.practice : {character: '', work: '', family: ''}
              that.pushData.classic = res.data.classic
              that.pushData.introspective = res.data.introspective
              that.pushCount = res.data.pushCount
              that.pushCount.num=0
              if(res.code==402){
                that.pushCount.num=that.pushCount.bonaDays
              }
              that.pushCount.bonaTotal=that.pushCount.bonaTotal-that.pushCount.num
              that.pushData.volunteer = res.data.volunteer
              that.books = res.data.bookish ? res.data.bookish : []
              that.booklength = that.bookss.length;
              let local=localStorage.getItem('cardPushData');
              if(local){
                that.pushData=JSON.parse(local);
                // let arr=[];
                // that.pushData.bookss.forEach((el,i) => {
                //     if( ! el.readCount == 0 ||el.type==1) {
                //       arr.push(el);
                //     }
                // })
                // that.pushData.bookss=arr;
                // if(res.code==402){
                //   that.pushData.pushCount.num=that.pushData.pushCount.bonaDays
                //   that.pushData.pushCount.bonaTotal=that.pushData.pushCount.bonaTotal-that.pushData.pushCount.num
                // }
                // return false
              }
              // else{
                // localStorage.setItem('cardPushData',JSON.stringify(that.pushData));
              // } 
          }
       })
    },
    punch(){
      let isUpdate=0;
      if(this.btnTitle == '修改') {
         isUpdate=1;
      }
      // if( this.repetition) {
      //    return false
      // }
      // this.repetition = true
      this.loading()
      pushCard({isUpdate:isUpdate,thanks:this.pushData.thanks,practice:this.pushData.practice,books:this.bookss,classic:this.pushData.classic,introspective:this.pushData.introspective,volunteer:this.pushData.volunteer,pushCount:this.pushCount,isPub:'1',bookish:this.books}).then( res => {
           if(res.code == 1) {
            //  this.repetition = false
             this.hideLoading()
             localStorage.removeItem('cardPushData');
              this.$router.push({path:'/nav/friend'})
           }
      })
    },
    cardBook () {
      let data={title:'',readCount:1,readTotal:0}
       this.books.push(data)
    },
    cardDel(i) {
       this.books.splice(i,1)
    },
    bblur() {
      window.scroll(0,0);
    },
  },
  created () {
     this.gainPer()
  },
  components: {
  },
  mounted() {
    window.addEventListener('resize', () => {
    const activeElement = document.activeElement
    if (activeElement.tagName === 'INPUT' || activeElement.tagName === 'TEXTAREA') {
        setTimeout(() => {
          // activeElement.scrollIntoView({block:'start'})
          // window.scroll(0,0);
          activeElement.scrollIntoViewIfNeeded()

        }, 0)
      }
    })
  },
  beforeDestroy() {
    window.removeEventListener('resize', () => {
    const activeElement = document.activeElement
    if (activeElement.tagName === 'INPUT' || activeElement.tagName === 'TEXTAREA') {
        setTimeout(() => {
          // activeElement.scrollIntoView({block:'start'})
          // window.scroll(0,0);
          activeElement.scrollIntoViewIfNeeded()

        }, 0)
      }
    })
  },

}
</script>
<style lang="less" >
.card{
   background: #F5F5F5;
    -webkit-overflow-scrolling : touch;
    position: relative;
.l-pu {
    height: 96px;
   font-size: 32px;
   display: flex;
   justify-content: space-between;
   text-align: center;
  .l-mi {
    width:180px;
    display: flex;
    align-items: center;
    .van-stepper__minus, .van-stepper__plus {
      width:33%;
      border-color: #ccc;
      height: 60px;
      background: #fff;
    }
    .van-stepper__minus {
      border-radius: 18px 0 0 18px;
    }
    .van-stepper__plus {
      border-radius: 0 18px 18px 0;
    }
    .van-stepper__input{
      line-height: 60px;
      width:33%;
      height: 60px;
      font-size: 24px;
      box-sizing: border-box;
       border:1PX solid #ccc;
      border-width:1PX 0;
      padding: 0;
      background: #fff;
    }
    .van-stepper__minus::before, .van-stepper__plus::before {
      width: 20px !important;
      height: 1PX !important;
      color: #999 !important;
    }
    .van-stepper__minus::after, .van-stepper__plus::after {
      height: 20px !important;
      width:1PX !important;
      color: #999 !important;
    }
    .van-stepper__minus, .van-stepper__plus{
      border:1PX solid #ccc;
    }
  }
}
.tit-box{
  width: 100%;
  border-bottom: 1PX solid #f2f2f2;
}
.l-put {
   height: 96px;
   font-size: 32px;
   display: flex;
   justify-content: flex-start;
   align-items: center;
   box-sizing: border-box;
   border-top: 1PX solid #F2F2F2;
   padding: 25px 0;
   .label {
     padding-left: 0px;
     color:#000;
   }
   input {
      height: 46px;
      line-height: 46px;
      width: 100%;
      text-align: right;
      background:none;
	    outline:none;
      border:0px;
      box-sizing: border-box;
      caret-color:blue;
   }
  .label-f {
    width: 280px;
  }
  .label-t {
    width: 220px;
  }
}
  .card-top {
     img {
       height: 100%;
       width: auto;
     }
  }
  .card-size {
    padding-left: 53px;
    color:#666;
    font-size: 28px;
  }
  .card-com {
    background: #fff;
    padding: 0 36px;
    height: 96px;
    line-height: 96px;
 }
 .laca {
    position: relative;
 }
 .laca-add {
   position: absolute;
   top: 50%;
   transform: translateY(-50%);
   height: 26px;
   left: -25%;
   img {
     height: 100%;
   }
 }
 .siz {
    position: absolute;
    top: 50%;
    transform:translateY(-50%);
    height: 30px;
    width: 33px;
 }
 .card-b{
    height: 80px;
    line-height: 80px;
    background-color: #f5f5f5;
    padding-left: 36px;
 }
 .card-book {
   color: #999;
   font-size: 26px;
 }
 .add-book {
   display: flex;
   justify-content: space-between;
   padding-right: 36px;
 }
 .del-book {
    color:#E62129;
 }
 .pdd {
    background: #fff;
    padding: 0 36px;
 }
 .put-bot{
   border-bottom: 1PX solid #F2F2f2;
 }
 .card-add {
    text-align: center;
    background: #fff;
    font-size: 33px;
    color:#E62129;
    font-weight: 500;
 }
 .card-share {
    font-size: 32px;
    color:#000;
    background: #fff;
    font-weight: 400;
    margin-top: 10px;
 }
 .card-put {
    font-size: 32px;
    background: #fff;
    padding: 33px;
    // height: ;
 }
 .card-ma {
    margin-top: 10px;
   display: flex;
   align-items: center;
 }
 .weui-cell {
   padding: 10px 36px;
 }
 .card-btn {
    margin-top: 110px;
    padding: 0 32px;
    padding-bottom: 32px;
    .card-div {
      height: 96px;
      font-size: 36px;
      color: #fff;
      background-color: #E62129;
      line-height: 96px;
      text-align: center;
      border-radius: 12px;
    }
 }
  .ccc {
     font-size: 32px;
     width: 100%;
     outline:none;
     border: none;
    //  padding:20px;
  }
  .tanks{
    border-bottom: 1PX solid #f2f2f2
  }
}
textarea{
  border-radius: 5px;
  padding: 10px;
}
textarea[disabled]{
  background: #f5f5f5;
}

</style>
