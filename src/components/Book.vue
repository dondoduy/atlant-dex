<template lang='pug'>
table.book
  tbody.book__body
    tr.book__row(v-for="order in orderBook")
      td.book__cell(:class="`book__cell--${(ask) ? 'ask' : 'bid'}`") {{order[0]}}
      td.book__cell {{order[1].toFixed(4)}}
      td.book__cell {{(order[0]*order[1]).toFixed(4)}}
</template>

<script>
import {mapState} from 'vuex';

export default {
  data() {
    return {
    };
  },
  computed: {
    ...mapState('trade', {
      book: (state) => state.book,
    }),
    orderBook() {
      const items = (this.ask) ? this.book.asks : this.book.bids;
      return items.slice(0, this.limit);
    },
  },
  props: {
    ask: {
      type: Boolean,
      default: false,
      required: false,
    },
    limit: {
      type: Number,
      default: 19,
      required: false,
    },
  },
};
</script>

<style lang="scss">
@import "~variables";
@import '~sass/bootstrap/media';

.book {
  width: 100%;
  &__cell {
    width: 33.333%;
    text-align: center;
    &--ask {
      color: $color_red;
    }
    &--bid {
      color: $color_green;
    }
  }
}

@include media-breakpoint-down(md) {
  .book {
    &__cell {
      text-align: center;
    }
  }
}
</style>
