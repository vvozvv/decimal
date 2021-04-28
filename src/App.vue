<template>
  <main class="main">
    <div class="decimal-list">
      <h1 class="decimal-list__title">Decimal</h1>
      <Decimal
        v-for="(item, index) in items"
        :key="index"
        :index="index"
        :showDeleteButton="(this.items.length >= 2) ? true : false"
        @calculateSum="changeValueArray"
        @delete="deleteItem"
      />
      <div class="decimal-box">
        <button class="btn" @click="toggleButton">{{ toggle ? 'Десятичное' : 'Обычное' }}</button>
        <p class="decimal-box__value" v-if="toggle">Итог: {{ result }}</p>
        <div class="decimal-value" v-else>
          <p class="decimal-value__value" v-show="decimalValue[0]">{{ decimalValue[0] }}</p>
          <div class="decimal-value__box">
            <p class="decimal-value__value decimal-value__value--border">{{ decimalValue[1] }}</p>
            <p class="decimal-value__value">{{ decimalValue[2] }}</p>
          </div>
        </div>
      </div>
      <div class="decimal-list__info">
        <button class="btn decimal-list__btn" @click="addNewFraction">Добавить новую дробь</button>
        <p class="decimal-list__count">Дробей: {{ items.length }} / {{ maxLengthItemsList }}</p>
      </div>
    </div>
  </main>
</template>
<script>
import Decimal from './components/Decimal'
export default {
  name: 'App',
  components: {
    Decimal,
  },
  data () {
    return {
      items: [0], 
      decimalValue: [],
      toggle: true,
      maxLengthItemsList: 5,
    }
  },
  methods: {
    addNewFraction() {
      (this.items.length <= this.maxLengthItemsList - 1) ? this.items.push(0) : ''
    },
    changeValueArray(val) {
      this.items[val.index] = +val.sum.toFixed(3)
    },
    deleteItem(index) {
      this.items.splice(index, 1)
    },
    toggleButton() {
      this.toggle = !this.toggle
    },
  },
  watch: {
    result() {
      this.decimalValue = []
      // Получаем первую часть
      let val = this.result.toString().split('.')

      const value = (val[1]) ? val[1].replace(/\.0+/) : ''

      // Количество чисел после запятой в обычной дроби
      let counZero = value.split('').length
      console.log('Чисел после запятой: ' + counZero)
      // Полцчаем знаменатель
      let znam = '1'
      for(let i = 0; i <= counZero; ++i) { znam += '0' }

      this.decimalValue.push(+val[0]) // Первая часть
      this.decimalValue.push(+value)   // Числитель
      this.decimalValue.push(+znam)    // Знаменатель

    }
  },
  computed: {
    result() {
      return this.items.reduce((total, item) => {
        return (item) ? total + item : total
      })
    },
  },
}
</script>
<style lang="scss">
  $color-accent: #FF5858;
  $ff-main: 'Source Sans Pro', sans-serif;
  
  * {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }
  body {
    font-family: $ff-main
  }
  input, textarea, button {
    outline: none;
    font-family: $ff-main;
    font-size: 16px;
  }
  .main {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100vw;
    height: 100vh;
    background: #FCF9F4;
  }
  .btn {
    $self: &;
    display: flex;align-items: center;justify-content: center;
    height: 50px;
    width: fit-content;
    background: transparent;
    border: 1px solid $color-accent;
    color: $color-accent;
    font-weight: 600;
    padding: 0 25px;
    transition: .2s ease;
    cursor: pointer;
    white-space: nowrap;
    position: relative;
    z-index: 15;
    font-size: 14px;
    overflow: hidden;
    &::after {
      position: absolute;
      content: '';
      left: 0;
      bottom: 0;
      width: 100%;
      height: 100%;
      background: $color-accent;
      transition: inherit;
      z-index: -1;
      transform: translateY(100%);
    }
    &:hover {
      opacity: .8;
      color: #fff;
      &::after {
        transform: translateY(0);
      }
    }
  }
  .decimal-list {
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    width: 100%;
    background: #fff;
    padding: 40px;
    max-width: 750px;
    position: relative;
    &__info {
      margin-top: 30px;
      display: flex;
      align-items: center;
      width: 100%;
    }
    &__count {
      margin-left: 20px;
    }
    &__title {
      position: absolute;
      top: -100px;
      font-weight: 700;
      color: #e7e7e7;
      font-size: 45px;
      left: 50%;
      transform: translateX(-50%);
    }
  }
  .decimal {
    $self: &;
    display: flex;
    flex-direction: column;
    width: fit-content;
    margin-right: 40px;
    position: relative;
    &::after {
      position: absolute;
      content: '+';
      right: -30px;
      top: 50%;
      font-size: 30px;
      font-weight: 700;
      transform: translateY(-50%);
    }
    &:nth-last-child(3) {
      &::after {
        content: '=';
      }
    }
    &--error {
      // border: 1px solid $color-accent;
      #{$self}__input {
        border: 1px solid $color-accent !important;
        background: #fff;
      }
    }
    &__input {
      border: 1px solid #f1f1f1;
      background: #f1f1f1;
      padding: 10px;
      width: 60px;
      margin-bottom: 10px;
      &:last-child {
        margin: 0;
      }
      &--border {
        position: relative;
        &::after {
          position: absolute;
          content: '';
          bottom: 0;
          left: 0;
          width: 100%;
          height: 1px;
          background: #212121;
        }
      }
    }
    &__btn {
      background: transparent;
      border: none;
      cursor: pointer;
    }
  }
  .decimal-value {
    display: flex;
    align-items: center;
    &__value {
      margin: 5px 0;
      text-align: center;
      &--border {
        width: 100%;
        margin: 5px 0 0 0;
        padding-bottom: 5px;
        border-bottom: 1px solid #212121;
      }
    }
    &__box {
      margin-left: 10px;
    }
  }
  .decimal-box {
    border: 1px solid #f1f1f1;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    &__value {
      padding: 5px 10px;
    }
  }
</style>