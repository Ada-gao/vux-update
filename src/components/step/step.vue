<template>
	<div class="vux-step">
    <slot></slot>
	</div>
</template>

<script>
export default {
  name: 'step',
  props: {
    value: Number,
    backgroundColor: {
      type: String,
      default: '#fff'
    },
    gutter: {
      type: String,
      default: '10px'
    }
  },
  created () {
    this.current = this.value
  },
  mounted () {
    this._mapPropsToChildComponent()
  },
  watch: {
    value (val) {
      this.current = val
    },
    current (val) {
      this._mapPropsToChildComponent()
      this.$emit('input', val)
    }
  },
  data () {
    return {
      current: 0
    }
  },
  methods: {
    _mapPropsToChildComponent () {
      const _this = this
      const len = this.$children.length - 1
      this.$children.forEach((child, index) => {
        child.currentStepNumber = (index + 1).toString()
        child.currentStepLast = index === len

        if (index === _this.current) {
          child.currentStatus = 'process'
        } else if (index < _this.current) {
          child.currentStatus = 'finish'
        } else {
          child.currentStatus = 'wait'
        }
      })
    }
  }
}
</script>

<style lang="less">
@import '../../styles/variable';
.vux-step {
  display: flex;
}
.vux-step-item {
  display: inline-block;
  position: relative;
  overflow: hidden;
}

.vux-step-item-with-tail {
  flex: 1;
}

.vux-step-item-tail {
  height: 4px;
  position: absolute;
  left: 0;
  top: 24px;
  padding: 0 0;
  transition: all 0.4s ease 0s;
}

.vux-step-item-tail-finish {
  background: @step-item-head-light-color none repeat scroll 0 0;
}

.vux-step-item-tail-process, .vux-step-item-tail-wait {
  background: @step-item-head-light-color none repeat scroll 0 0;
}

.vux-step-item-icon {
  width: 22px;
  height: 22px;
  display: inline-block;
  text-align: center;
}

.vux-step-item-checked::before {
  font-size: 15px!important;
  line-height: 22px;
  margin: 0!important;
  transform: translateY(-4px);
}

.vux-step-item-title {
  font-size: 0.8rem;
}

.vux-step-item-head {
  position: relative;
  display: inline-block;
  margin-right: -4px;

  .vux-step-item-head-inner {
    width: 22px;
    height: 22px;
    line-height: 22px;
    border-radius: 99px;
    text-align: center;
    font-size: 14px;
    transition: all 0.4s ease 0s;
    background: #fff none repeat scroll 0 0;
  }
}

.vux-step-item-head-finish .vux-step-item-head-inner{
  border: 4px solid @step-item-head-light-color;
  color: @step-item-head-color;
}

.vux-step-item-head-process .vux-step-item-head-inner{
  border: 4px solid @step-item-head-light-color;
  color: #FFF;
  background: @step-item-head-color none repeat scroll 0 0;
}

.vux-step-item-head-wait .vux-step-item-head-inner {
  border: 4px solid @step-item-head-light-color;
  color:  @step-item-head-color;
}

.vux-step-item-main {
  display: inline-block;
  position: relative;
  vertical-align: top;
  color: #888;
  padding-left: 5px;
}

.vux-step-item-main-process {
  font-weight: bold;
  color: #666;
}
// 修改样式
.vux-step-item-head .vux-step-item-head-inner {
  width: 46px;
  height: 46px;
  line-height: 46px;
}
.vux-step-item-icon {
  font-size: 28px;
}
.vux-step-item-tail {
  left: 57%;
  width: 40px;
}
.vux-step-item-description {
  position: absolute;
  left: -50%;
  transform: translateX(50%);
  white-space: nowrap;
  color: @step-item-head-color;
}
.vux-step-item {
  overflow: visible;
  &:nth-child(1) .vux-step-item-description {
    transform: translateX(15%);
  }
  &:nth-last-child(1) .vux-step-item-description {
    transform: translateX(0);
  }
}
.vux-step-item-main {
  display: block;
}
.vux-step-item-main-wait,
.vux-step-item-main-finish {
  display: none;
}
.vux-step {
  height: 110px;
}
.weui-cells__title {
  height: 90px;
  line-height: 90px;
  font-size: 26px!important; /*px*/
  color: #999!important;
}
</style>
