<template>
<!--  <div class="adCard v-card mt-12">
    <div class="ad" v-html="adLink" />
  </div>-->
  <div class="ad" v-html="adLink" />
</template>

<script>
export default {
  name: "AdCard",
  computed: {
    adLink () {
      return '<iframe src="//rcm-na.amazon-adsystem.com/e/cm?o=1&p=288&l=ur1&category=primevideo&banner=121YK2TVQY3WK6QWFBR2&f=ifr&linkID=9a912221d6d751fbfa9685a082f45188&t=jlapps08-20&tracking_id=jlapps08-20" width="320" height="50" scrolling="no" border="0" marginwidth="0" style="border:none;" frameborder="0" sandbox="allow-scripts allow-same-origin allow-popups allow-top-navigation-by-user-activation"></iframe>'
    }
  },
}
</script>
