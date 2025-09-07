<template>
	<div v-if="show && money" class="tui-amount-inwords"
		:style="{fontSize:size+'rpx',color:color,fontWeight:fontWeight,padding:padding}">{{money}}</div>
</template>

<script setup>
import { ref, watch, onMounted } from "vue";

// 定义 props
const props = defineProps({
  value: {
    type: [Number, String],
    default: 0
  },
  size: {
    type: [Number, String],
    default: 32
  },
  color: {
    type: String,
    default: '#333'
  },
  fontWeight: {
    type: [Number, String],
    default: 400
  },
  padding: {
    type: String,
    default: '0'
  },
  show: {
    type: Boolean,
    default: true
  }
});

// 定义 emits
const emit = defineEmits(['change']);

// data -> ref
const money = ref("");

// 方法
function trimAll(value) {
  return value.toString().replace(/\s+/g, "").replace('￥', "");
}

function getAmountInWords(moneyVal) {
  moneyVal = trimAll(moneyVal);
  let cnNums = ['零', '壹', '贰', '叁', '肆', '伍', '陆', '柒', '捌', '玖'];
  let cnIntRadice = ['', '拾', '佰', '仟'];
  let cnIntUnits = ['', '万', '亿', '兆'];
  let cnDecUnits = ['角', '分', '毫', '厘'];
  let cnInteger = '整';
  let cnIntLast = '元';
  let maxNum = 999999999999999.9999;
  let integerNum;
  let decimalNum;
  let chineseStr = '';
  let parts;

  if (moneyVal === '') return '';
  moneyVal = parseFloat(moneyVal);
  if (moneyVal >= maxNum) return '';
  if (moneyVal === 0) return cnNums[0] + cnIntLast + cnInteger;

  moneyVal = moneyVal.toString();
  if (moneyVal.indexOf('.') === -1) {
    integerNum = moneyVal;
    decimalNum = '';
  } else {
    parts = moneyVal.split('.');
    integerNum = parts[0];
    decimalNum = parts[1].substr(0, 4);
  }

	// 1205 => 壹仟贰佰零伍元整
  if (parseInt(integerNum, 10) > 0) {
    let zeroCount = 0;
    let IntLen = integerNum.length;  //    4
    for (let i = 0; i < IntLen; i++) {  // 0  1  2  3
      let n = integerNum.substr(i, 1);  // 1  2  0  5
      let p = IntLen - i - 1;  //          3  2  1  0
      let q = Math.floor(p / 4);  //       0  0  0  0
      let m = p % 4;  //                   3  2  1  0
      if (n === '0') {
        zeroCount++;                 //    1
      } else {
        if (zeroCount > 0) {
          chineseStr += cnNums[0];  //   零
        }
        zeroCount = 0;
        chineseStr += cnNums[parseInt(n)] + cnIntRadice[m];  //  壹仟贰佰零零拾五
      }
      if (m === 0 && zeroCount < 4) {
        chineseStr += cnIntUnits[q];
      }
    }
    chineseStr += cnIntLast;//  壹仟贰佰零零拾五元
  }

  if (decimalNum !== '') {
    let decLen = decimalNum.length;
    for (let i = 0; i < decLen; i++) {
      let n = decimalNum.substr(i, 1);
      if (n !== '0') {
        chineseStr += cnNums[Number(n)] + cnDecUnits[i];
      }
    }
  }

  if (chineseStr === '') {
    chineseStr += cnNums[0] + cnIntLast + cnInteger;
  } else if (decimalNum === '') {
    chineseStr += cnInteger;
  }
  return chineseStr;
}

// watch 替代
watch(
  () => props.value,
  (newValue) => {
    money.value = getAmountInWords(newValue);
    emit('change', { moeny: money.value });
  },
  { immediate: true } // 相当于 created 时初始化
);

// mounted 时手动触发（可选，因为 watch immediate 已处理）
onMounted(() => {
  money.value = getAmountInWords(props.value);
  emit('change', { moeny: money.value });
});
</script>


<style scoped></style>
