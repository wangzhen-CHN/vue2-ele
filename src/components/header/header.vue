<template>
  <div class="header" ref="headerWrapperDom">
    <div class="content-wrapper" ref="contentWrapperDom">
      <div class="avatar" ref="avatarDom">
        <img :src="seller.avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description" ref="descriptionDom">
          {{seller.description}}/{{seller.deliveryTime}}分钟到达
        </div>
        <div class="support" v-if="seller.supports" ref="supportDom">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="supports_count" v-if="seller.supports" @click="showDetail(true)">
        <span class="count">{{seller.supports.length}}个</span>
        <span class="icon-keyboard_arrow_right"></span>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail(true)" ref="bulletinWrapperDom">
      <span class="bulletin-title"></span>
      <span class="bulletin-text">{{seller.bulletin}}</span>
      <span class="icon-keyboard_arrow_right"></span>
    </div>
    <div class="background">
      <img :src="seller.avatar">
    </div>
    <transition name="fade">
      <div class="detail" v-show="detailShow">
        <div class="detail-wrapper">
          <div class="detail-main">
            <div class="name">{{seller.name}}</div>
            <div class="star-wrapper">
              <star :score="seller.score" :size="48"></star>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul class="supports">
              <li class="support" v-for="support in seller.supports" :key="support.type">
                <span class="icon" :class="classMap[support.type]"></span>
                <span class="text">{{support.description}}</span>
              </li>
            </ul>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div class="content">
              <p>{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="showDetail(false)">
          <span class="icon-close"></span>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
  import star from '../star/star.vue';
  import bus from '../../assets/eventBus';
  export default {
    props: {
          seller: Object
    },
    data () {
      return {
        detailShow: false
      }
    },
    created () {
      this.classMap = ["decrease", "discount", "guarantee", "invoice", "special"]
    },
    mounted(){
    
      var headerWrapperDom = this.$refs.headerWrapperDom;
      var avatarDom = this.$refs.headerWrapperDom.getElementsByClassName('avatar');
      var contentWrapperDom = this.$refs.headerWrapperDom.getElementsByClassName('content-wrapper');
      var bulletinWrapperDom = this.$refs.headerWrapperDom.getElementsByClassName('bulletin-wrapper');
      var descriptionDom = this.$refs.headerWrapperDom.getElementsByClassName('description');
      var supportDom = this.$refs.headerWrapperDom.getElementsByClassName('support');
console.log(contentWrapperDom[0]);
        bus.$on('userBus', function(scrollY) {
        
         avatarDom[0].style.opacity=1-scrollY/130;
         descriptionDom[0].style.opacity=1-scrollY/130;
         supportDom[0].style.opacity=1-scrollY/130;
         contentWrapperDom[0].style.paddingTop=(1-scrollY/300)*24+'px';
         contentWrapperDom[0].style.marginLeft=(-scrollY/140)*80+'px';
         bulletinWrapperDom[0].style.opacity=1-scrollY/140;
         avatarDom[0].style.width=(1-scrollY/800)*64+'px';
         avatarDom[0].style.height=(1-scrollY/800)*64+'px';
         headerWrapperDom.style.height=(1-scrollY/200)*140+'px';
      });       
    },
    methods: {
      showDetail (isShow) {
        this.detailShow = isShow
      }
    },
    components: {
      star
    }
  }
</script>

<style lang="less">
  @import "../../common/style/mixins.less";
  .header {
  position: relative;
  background: rgba(7,17,27,0.5);
  color: #fff;
  overflow: hidden;
  .content-wrapper {
    position: relative;
    padding: 24px 12px 18px 24px;
    display: flex;
    .avatar {
      vertical-align: top;
      display: inline-block;
      width: 64px;
      height: 64px;
      border: 1px solid #aaa;
      img {
        width: 100%;
      }
    }
    .content {
      display: inline-block;
      margin-left: 16px;
      padding: 2px 0 2px;
      flex: 1;
      .title {
        .brand {
          display: inline-block;
          width: 30px;
          height: 18px;
          background-repeat: no-repeat;
          background-size: 30px 18px;
          vertical-align: middle;
          .bg-image(header,brand);
        }
        .name {
          margin-left: 6px;
          font-size: 16px;
          line-height: 18px;
          font-weight: bold;
        }
      }
      .description {
        margin-top: 8px;
        margin-bottom: 10px;
        font-size: 12px;
        font-weight: 200;
        line-height: 12px;
      }
      .support {
        .icon {
          display: inline-block;
          width: 12px;
          height: 12px;
          background-size: 12px 12px;
          background-repeat: no-repeat;
          vertical-align: middle;
        }
        .decrease{
            .bg-image(header,decrease_1);
            }
          .discount{
            .bg-image(header,discount_1);
            }
          .guarantee{
            .bg-image(header,guarantee_1);
            }
          .invoice{
            .bg-image(header,invoice_1);
            }
          .special{
            .bg-image(header,special_1);
            }
        .text {
          margin-left: 4px;
          font-size: 10px;
          line-height: 12px;
          font-weight: 200;
        }
      }
    }
    .supports_count {
      background: rgba(0,0,0,0.2);
      border-radius: 8px;
      position: absolute;
      right: 12px;
      bottom: 18px;
      height: 24px;
      padding: 4px 8px;
      box-sizing: border-box;
      .count {
        font-size: 10px;
        line-height: 12px;
        font-weight: 200;
        vertical-align: middle;
      }
    }
  }
  .bulletin-wrapper {
    position: relative;
    padding: 0 12px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    height: 28px;
    line-height: 28px;
    background: rgba(7,17,27,0.2);
    .bulletin-title {
      display: inline-block;
      width: 22px;
      height: 12px;
      background-repeat: no-repeat;
      background-size: 22px 12px;
      vertical-align: middle;
      .bg-image(header,bulletin)
    }
    .bulletin-text {
      margin: 0 4px;
      font-size: 10px;
      font-weight: 200;
    }
    .icon-keyboard_arrow_right {
      position: absolute;
      top: 9px;
      right: 2px;
    }
  }
  .background {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: -1;
    filter: blur(40px);
    img {
      width: 100%;
      height: 100%;
    }
  }
  .detail {
    position: fixed;
    height: 100%;
    display: flex;
    flex-direction: column;
    overflow-y: scroll;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(7,17,27,0.9);
    z-index: 100;
    &.fade-enter-active, &.fade-leave-active{
        transition: opacity .3s linear;
      &.fade-enter,&.fade-leave-to{
        opacity: 0;              
      }            
    }
    .detail-wrapper {
      flex: 1;
      padding-top: 64px;
      .detail-main {
        .name {
          text-align: center;
          font-size: 16px;
          line-height: 16px;
          font-weight: 700;
          margin-bottom: 16px;
        }
        .star-wrapper {
          height: 24px;
          text-align: center;
        }
        .title {
          padding: 28px 36px 24px;
          display: flex;
          .line {
            position: relative;
            top: 8px;
            flex: 1;
            width: 107px;
            height: 2px;
            background: rgba(255,255,255,0.2);
          }
          .text {
            float: left;
            margin: 0 12px;
          }
        }
        .supports {
          padding: 0 48px;
          .support {
            margin-bottom: 12px;
            .icon {
              display: inline-block;
              width: 16px;
              height: 16px;
              background-size: 16px 16px;
              background-repeat: no-repeat;
              vertical-align: middle;
            }
            .decrease{
              .bg-image(header,decrease_2);
            }
            .discount{
              .bg-image(header,discount_2);
              }
            .guarantee{
              .bg-image(header,guarantee_2);
              }
            .invoice{
              .bg-image(header,invoice_2);
              }
            .special{
              .bg-image(header,special_2);
              }
            .text {
              margin-left: 6px;
              font-size: 12px;
              font-weight: 200;
              line-height: 12px;
            }
          }
        }
        .content {
          padding: 0 48px;
          font-size: 12px;
          font-weight: 200;
          line-height: 24px;
        }
      }
    }
    .detail-close {
      text-align: center;
      line-height: 80px;
      font-size: 26px;
      height: 80px;
      color: rgba(255,255,255,0.5);
    }
    .icon-close{
      font-size: 22px;
    }
  }
}
.header .detail.fade-enter-active,
.header .detail.fade-leave-active {
  transition: opacity 0.3s linear;
}
.header .detail.fade-enter,
.header .detail.fade-leave-to {
  opacity: 0;
}

</style>