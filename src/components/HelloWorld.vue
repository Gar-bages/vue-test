<template>
  <div class="level-entry">
    <div class="level-entry-content" :style="{backgroundImage:`url(${it.bgImg})`}" v-for="(it, index) in cardData" :key="index">
      <div class="content-head">
        <span>{{it.e_title}}</span>
      </div>
      <div class="content-title">
        {{it.c_title}}
      </div>
      <ul class="content-explain">
        <li v-for="(val, key) in it.tips" :key="key">{{val}}</li>
      </ul>
      <div v-if="it.participateNum > 0" class="reset-container">
        <div class="line"></div>
        <div class="info">
          <div>{{it.text}}：<span>{{it.rank}}</span></div>
          <div @click="routeTo(it.historyPath)">
            <span>历史记录</span>
            <img src="~@/assets/arrow.png"/>
          </div>
        </div>
      </div>
      <button v-if="it.participateNum !== -1" class="content-button" @click="routeTo(it.checkPath(it.rank))">{{it.participateNum > 0 ? '重新测评' : '开始测评'}}</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'level-entry',
  data () {
    return {
      disabled: true,
      cardData: [
        {
          e_title: 'VOCABULARY',
          c_title: '词汇测评',
          tips: ['最全面英语语料库', '最智能AI选题', '最权威国际标准'],
          text: '当前单词量',
          historyPath: '/history?adaptiveType=0',
          checkPath: (rank) => `/level?vocabulary=${rank || 0}`,
          bgImg: require('@/assets/entry1.png'),
          createTime: '',
          participateNum: '',
          rank: '',
          adaptiveType: 0
        },
        {
          e_title: 'READING COMPREHENSION',
          c_title: '阅读测评',
          tips: ['以蓝思值 (L) 衡量适合阅读的语料难度', '明确学生最适合阅读的文章难度', '对标CEFR、中国课标，解析学生的阅读水平'],
          text: '适合阅读难度',
          historyPath: '/history?adaptiveType=1',
          checkPath: () => `/read`,
          bgImg: require('@/assets/entry2.png'),
          createTime: '',
          participateNum: '',
          rank: '',
          adaptiveType: 1
        }
      ]
    }
  },
  mounted () {
    // CheckFirst().then(res => {
    //   let data = res.data.sort((a, b) => a.adaptiveType - b.adaptiveType);
    //   this.cardData.forEach((val) => {
    //     let info = data.filter(it => it.adaptiveType === val.adaptiveType) || [];
    //     if (info.length > 0) {
    //       let rank = info[0].rank;
    //       // 如果是阅读测评单独处理
    //       if (info[0].adaptiveType === 1) {
    //         rank = this.newRankObj(info[0].rank)
    //       }
    //       Object.assign(val, info[0], {rank});
    //     }
    //   })
    //   this.disabled = false;
    // }).catch(() => {
    //   this.$messagebox({
    //     title: ' ',
    //     message: `请求失败，再次加载？`,
    //     showCancelButton: true,
    //     confirmButtonText: '确认',
    //     cancelButtonText: '取消',
    //     confirmButtonPosition: 'right'
    //   }).then(t => {
    //     t === 'confirm' && window.location.reload();
    //   })
    //   this.disabled = false;
    // });
  },
  methods: {
    routeTo (path) {
      this.$router.push(path);
    },
    // 阅读测评 -- 当前蓝思值的区间
    newRankObj(rank) {
      let min = Math.floor(rank / 50) * 50;
      let max = min + 50;
      return `${min}-${max}L`
    }
  }
}
</script>

<style lang="less" scoped>
.level-entry {
  height: 100%;
  box-sizing: border-box;
  padding-top: 25px;
  font-family: PingFangSC-Semibold;
  &-content {
    width: 347px;
    min-height:275px;
    margin: 0 auto;
    padding: 20px 24px 24px;
    box-sizing: border-box;
    box-shadow: 0 6px 8px 0 rgba(109,114,120,0.10), 0 2px 10px 0 rgba(0,0,0,0.05);
    border-radius: 12px;
    background-color: #ffffff;
    background-size: cover;
    margin-bottom: 16px;
    .content-head {
      span {
        font-size: 12px;
        font-family: PingFangSC, PingFangSC-Regular;
        font-weight: 400;
        color: #464c54;
      }
    }
    .content-title {
      margin-bottom: 20px;
      font-size: 28px;
      font-family: PingFangSC, PingFangSC-Semibold;
      font-weight: 600;
      color: #2f3338;
      line-height: 40px;
    }
    .content-explain {
      font-size: 14px;
      font-family: PingFangSC, PingFangSC-Regular;
      font-weight: 400;
      color: #2f3338;
      li {
        margin-bottom: 10px;
      }
      li::before {
        content: '';
        width: 6px;
        height: 6px;
        margin-right: 6px;
        opacity: 0.66;
        display: inline-block;
        border-radius: 3px;
        background: #F5A623;
      }
    }
    .content-button {
      width: 295px;
      height: 50px;
      background: #fa3232;
      border-radius: 8px;
      font-size: 16px;
      font-family: PingFangSC, PingFangSC-Semibold;
      font-weight: 600;
      color: #ffffff;
      margin-top: 20px;
      &:active {
        background-color: #E02C2C;
      }
    }
    .info-icon {
      width: 14px;
      margin-left: 4px;
    }
  }
  .reset-container {
    padding: 14px 0 4px 0;
    .line {
      height: 1px;
      background: rgba(0,0,0,0.10);
      border-radius: 20px;
      margin-bottom: 20px;
    }
    .info {
      display: flex;
      justify-content: space-between;
      align-items: center;
      div:nth-of-type(1) {
        font-size: 14px;
        font-family: PingFangSC, PingFangSC-Medium;
        font-weight: 500;
        text-align: left;
        color: #2f3338;
        line-height: 20px;
      }
      div:nth-of-type(2) {
        display: flex;
        align-items: center;
        font-size: 12px;
        font-family: PingFangSC, PingFangSC-Regular;
        font-weight: 400;
        color: #69727d;
        line-height: 17px;
        img {
          width: 16px;
          vertical-align: middle;
        }
      }
    }
  }
}
</style>