<template>
  <article class="cart-container">
    <header>
      <h2 class="cart-title"> {{ title }} </h2>
    </header>
    <div class="cart-items-container">
      <ul class="item-cols-wrapper items-cols-bar">
        <li class="col-check-state">
          <span class="btn-check btn-check-all" 
            :class="{checked: checkedAll}"
            @click="switchAllBooksCheckState(checkedAll)">
          </span>
          <span class="items-bar-title">全选</span>
        </li>
        <li class="col-book-info">
          <span class="items-bar-title">图书</span>
        </li>
        <li class="col-price">
          <span class="items-bar-title">价格</span>
        </li>
        <li class="col-edit">
          <span class="items-bar-title">编辑</span>
        </li>
      </ul>
      <ul class="items-container">
        <li class="item-container" 
          v-for="book of cartBooks" 
          :key="book.id">
          <ul class="item-cols-wrapper">
            <li class="col-check-state">
              <span class="btn-check btn-check-item" 
                :class="{checked: book.checked}"
                @click="switchBookCheckState(book)">
              </span>
            </li>
            <li class="col-book-info">
              <div class="image-wrapper">
                <img class="book-image" 
                  :src="book.images.small" 
                >
              </div>
              <div class="book-info-wrapper">
                <h4 class="book-title"> {{ book.title }} </h4>
                <p class="book-author"> {{ book.author.join(',') }} </p>
              </div>
            </li>
            <li class="col-price">
              <span class="price"> {{ book.price }} </span>
            </li>
            <li  class="col-edit">
              <img class="btn-delete" 
                src="../../assets/images/ic_delete.png" 
                alt="删除"
                @click="deleteBookFromCart(book)"
                >
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <footer>
      <div class="settle-account-wraper">
        <div>
          共 <span class="total-count">{{ amountOfCheckedBooks }}</span> 本，
        </div>
        <div>
          应付金额 <span class="total-price">{{ totalPrice }}</span>
        </div>
        <div>
          <a href="javascript:;"
            class="btn-settle-account"
            @click="settleAccount">
            <span>结算</span>
          </a>
        </div>
      </div>
    </footer>
  </article>
</template>

<script>
  import {mapGetters, mapActions} from 'vuex'

  export default {
    name: 'Cart',
    data () {
      return {
        title: '购物车状态'
      }
    },
    computed: {
      ...mapGetters([
        'cartBooks',
        'checkedAll'
      ]),
      amountOfCheckedBooks() {
        return this.$store.getters.amountOfCheckedBooks();
      },
      totalPrice() {
        return this.$store.getters.totalPriceOfCheckedBooks();
      }
    },
    methods: {
      ...mapActions([
        'deleteBookFromCart',
        'clearCart',
        'switchBookCheckState',
        'switchAllBooksCheckState'
      ]),
      settleAccount() {
        alert('下单成功！');
        this.clearCart();
      }
    }
  }
</script>

<style lang="scss" scoped>
ul {
  list-style: none;
}

.cart-container {
  width: 958px;
  margin: 2rem auto;
  .cart-title {
    padding: 1rem;
  }
}

.cart-items-container {
  border: 1px solid #e0dad5;
}

.item-cols-wrapper {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  padding: .5rem;
  align-items: center;
}

.col-check-state {
  width: 15%;
  padding: 1rem;
  .btn-check {
    display: inline-block;
    width: 16px;
    height: 16px;
    background: url(../../assets/images/ic_check.png);
  }
  .btn-check:hover {
    cursor: pointer;
  }
  .checked {
    background: url(../../assets/images/ic_checked.png);
  }
}
.col-book-info {
  width: 60%;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  .image-wrapper {
    padding: .2rem;
    img {
      width: 48px;
      height: 64px;
    }
  }
  .book-info-wrapper {
    padding: .5rem 1rem;
    .book-title {
      font-size: .9rem;
      font-weight: normal;
      padding-bottom: .5rem;
      color: #613f23;
    }
    .book-author {
      font-size: .7rem;
      color: #999;
    }
  }
}
.col-price {
  width: 15%;
  padding: 1rem;
}
.col-edit {
  width: 10%;
  padding: 1rem;
}

.items-cols-bar {
  border-bottom: 1px solid #e0dad5;
  background: #f7f5f1;
}

.settle-account-wraper {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  padding: 2rem;
  background: #f7f5f1;
  .total-count, .total-price {
    padding: 0 .2rem;
    color: rgb(255, 122, 32);
  }
  .btn-settle-account {
    color: rgb(255, 255, 255);
    padding: .5rem 3rem;
    margin-left: 2rem;
    background: rgb(225, 79, 55);
  }
}
</style>