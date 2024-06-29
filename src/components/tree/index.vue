<template>
  <div :class="`container ${isLeaf ? 'leaf' : ''} ${isRoot ? 'root' : ''}`">
    <!-- 自身信息展示 -->
    <div class="self">
      <!-- 头部进度条 -->
      <div :class="`progress-container  ${idx === 0 ? 'first' : ''}`">
        <!-- 实时票款差 -->
        <div class="progress-item" :style="`--background: #409EFF;--percent: ${pay_invoice_percent}%`">
          <div class="label">实时票款差</div>
          <div class="degree">
            <div class="degree-inner"></div>
            <div class="degree-percent">{{ pay_invoice_percent }}%</div>
          </div>
          <div class="percent">{{ pay_invoice_percent }}%</div>
        </div>
        <!-- 票 -->
        <div class="progress-item" :style="`--background: #67C23A;--percent: ${invoice_percent}%`">
          <div class="label">票</div>
          <div class="degree">
            <div class="degree-inner"></div>
            <div class="degree-percent">{{ invoice_percent }}%</div>
          </div>
          <div class="percent">{{ invoice_percent }}%</div>
        </div>
        <!-- 款 -->
        <div class="progress-item" :style="`--background: #F56C6C;--percent: ${pay_percent}%`">
          <div class="label">款</div>
          <div class="degree">
            <div class="degree-inner"></div>
            <div class="degree-percent">{{ pay_percent }}%</div>
          </div>
          <div class="percent">{{ pay_percent }}%</div>
        </div>
      </div>
      <!-- 分割线 -->
      <div class="splitContainer">
        <div class="companyName" v-if="idx === 0">
          <div class="circle"></div>
          <div class="name">{{ info.contract_party_a_company_name  }}</div>
        </div>
        <div class="splitLine"></div>
      </div>
      <!-- 下半区 -->
      <div :class="`btmContainer ${idx === floorLength ? 'last' : ''}`">
        <div v-if="idx === 0">
          <span class="contractA" v-if="isRoot">甲方</span>
          <div class="amountContainer" v-else>
            <div class="amountRow">
              <div class="amountLabel">账户余额:</div>
              <div class="amountValue">{{ info?.contract_money }}</div>
            </div>
            <div class="amountRow">
              <div class="amountLabel">账户余额:</div>
              <div class="amountValue">{{ info?.contract_money }}</div>
            </div>
            <div class="amountRow">
              <div class="amountLabel">账户余额:</div>
              <div class="amountValue">{{ info?.contract_money }}</div>
            </div>
          </div>
        </div>
        <div v-else></div>
        <div class="actContainer">
          <div class="contractPrice">
            <span>合同金额：{{ info?.contract_money }}(税率：{{ info?.tariff }}%)</span>
            <button>查看</button>
          </div>
          <div class="btnContainer">
            <div class="btnRow">
              <div class="btnRowLabel">合同</div>
              <div class="btnList">
                <button class="btn">变更</button>
                <button class="btn">作废</button>
                <button class="btn">签署</button>
              </div>
            </div>
            <div class="btnRow">
              <div class="btnRowLabel">审批</div>
              <div class="btnList">
                <button class="btn">开票</button>
                <button class="btn">付款</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="childrenContainer">
      <lcc-tree v-for="(k, i) in info?.children" :key="i" :info="k" :isRoot="false" :idx="i"
      :floorLength="info?.children?.length - 1"
      ></lcc-tree>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LccTree',
  props: {
    info: {
      default: undefined
    },
    isRoot: {
      default: false
    },
    idx: {
      default: 0
    },
    /** 本层深度, {0,length-1} */
    floorLength: {
      default: 0
    }
  },
  created() {
    console.log('son-info', this.info)
  },
  data() {
    return {
    }
  },
  computed: {
    isLeaf() {
      return !this.info?.children || this.info?.children?.length === 0
    },
    pay_invoice_percent() {
      if (this.info?.payment_sum === 0) {
        return 0
      }
      const _diff = this.info?.payment_sum - this.info?.invoice_sum
      return Math.floor(_diff / this.info?.payment_sum * 100)
    },
    invoice_percent() {
      return Math.floor(this.info?.invoice_sum / this.info?.contract_money * 100)
    },
    pay_percent() {
      return Math.floor(this.info?.payment_sum / this.info?.contract_money * 100)
    }
  }
}
</script>

<style lang="scss" scoped>
@import './index.scss';
</style>