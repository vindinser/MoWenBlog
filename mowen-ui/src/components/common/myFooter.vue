<template>
  <div class="myFooter-wrap" v-show="showFooter">
    <div class="myFooter">
      <div class="footer-title">{{$store.state.webInfo.footer}}</div>
      <div class="icp">
        <span>
          <span>本网站由</span>
          <a href="https://gitee.com/vindinser/mo-wen-blog" target="_blank">ZhangShuang</a>
          <span>强力支持</span>
        </span>
        <span class="beian-group" v-if="$store.state.sysConfig.gonganbeian">
          <img src="@/assets/beian.png" alt="">
          <a href="https://beian.mps.gov.cn/#/query/webSearch?code=13082502000149" rel="noreferrer" target="_blank">
            {{ $store.state.sysConfig.gonganbeian }}
          </a>
        </span>
        <span class="beian-group" v-if="$store.state.sysConfig.gonganbeian">
          <a href="http://beian.miit.gov.cn/" target="_blank">{{ $store.state.sysConfig.beian }}</a>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      showFooter: {
        type: Boolean,
        default: true
      }
    },
    data() {
      return {}
    },
    created() {
    }
  }
</script>

<style scoped>
  .myFooter-wrap {
    user-select: none;
    animation: hideToShow 2s;
  }

  .myFooter {
    border-radius: 1.5rem 1.5rem 0 0;
    background: var(--gradientBG);
    text-align: center;
    color: var(--white);
    background-size: 300% 300%;
    animation: gradientBG 10s ease infinite;
  }

  .footer-title {
    padding-top: 10px;
    font-size: 16px;
  }

  .icp, .icp a {
    color: var(--maxGreyFont);
    font-size: 13px;
    white-space: nowrap;
  }

  .icp {
    padding-top: 10px;
    padding-bottom: 10px;
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;
    gap: 4px;
    text-align: center;
    max-width: 100%;
    box-sizing: border-box;
  }

  .icp a {
    text-decoration: none;
    transition: all 0.3s;
  }

  .icp a:hover {
    color: var(--themeBackground);
  }

  .icp img {
    width: 16px;
    height: 16px;
    vertical-align: middle;
    margin-left: 10px;
    margin-right: -2px;
  }

  .beian-group {
    white-space: nowrap; /* 默认防止内部换行 */
    display: flex;
    align-items: center;
    gap: 4px; /* 图片和文字之间的间距 */
  }

  @media (max-width: 400px) {
    .icp {
      flex-direction: column;
      align-items: center;
      gap: 8px;
    }
    
    .icp > .beian-group:nth-of-type(2) {
      order: 2;
    }
    
    .beian-group {
      display: flex;
      justify-content: center;
      white-space: normal;
    }
  }
</style>
