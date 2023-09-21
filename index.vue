<template>
  <div :id="id" class="block cards-block"
    :class="[ classes, { 'animate js_section': setAnimate || animate == 'true', 'visible': visible == true } ]">

    <div class="text-wrap" v-if="title">
      <h2>{{ title }}</h2>
    </div>

    <div class="cards">
      <component
        :is="card.link?'a':'div'"
        :href="card.link || ''"
        :target="card.target"
        v-for="(card, key) in allCards"
        :key="key"
        ref="card"
        class="card"
        :class="[
          card.class,
          {
            'text-hover': card.textHover,
            'hide-text': card.hideText,
            'visible': card.visible
          }
        ]"
      >
        <div class="image-wrap" v-if="card.image">
          <img :src="card.image" :alt="card.title" loading="lazy" />
        </div>

        <div class="image-wrap svg-wrap" v-if="card.svg">
          <nuxt-icon :name="card.svg" filled />
        </div>

        <div class="text-wrap inner-container">
          <h3>{{ card.title }}</h3>
          <p v-if="card.subtitle">{{ card.subtitle }}</p>
          <Buttons :buttons="card.buttons" />
        </div>

        <div class="tooltip" v-if="card.tooltip">{{ card.tooltip }}</div>
      </component>
    </div>

  </div>
</template>

<script>
export default {
  props: [
    'animate',
    'visible',
    'classes',
    'id',
    'title',
    'cards',
  ],
  data() {
    return {
      setAnimate: false,
      allCards: [...this.cards],
    }
  },
  watch: {
    visible(value) {
      if (value == true) {
        const observer = new IntersectionObserver((entries, observer) => {
          entries.forEach((entry, i) => {
            if (entry.isIntersecting) {
              this.allCards[entry.target.index].visible = true
            }
          });
        }, {
          threshold: 0,
          rootMargin: "0px 0px 50px 0px",
        });

        this.$refs.card.forEach( function (el, index) {
          el.index = index
          observer.observe(el);
        })
      }
    }
  },
}
</script>

<style lang="less">
  @import 'style';
</style>