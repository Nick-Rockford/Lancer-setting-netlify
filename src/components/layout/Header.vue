<template>
  <header class="clipped-x-large-forward">
    <div class="title clipped-x-large-forward">
      <img class="logo" src="/icons/company-icon.svg" />
      <div class="title-container">
        <div class="title-row" id="title-first-line">
          <span id="title-header">{{ header.headerTitle }}</span>
          <span id="title-subheader">{{ header.headerSubtitle }}</span>
        </div>
        <div class="title-row">
          <span id="subtitle-header">{{ header.subheaderTitle }}</span>
          <span id="subtitle-subheader">// {{ header.subheaderSubtitle }}</span>
        </div>
      </div>
    </div>
    <div class="rhombus">&nbsp;</div>
    <video autoplay muted loop width="90px" height="90px">
      <source src="/planet.webm" type="video/webm" />
    </video>
    <div class="location-info">
      <div class="location-row" id="planet-year">
        <div id="planet">
          <h4>Deployment Info</h4>
          <span class="subtitle">{{ header.planet }}</span>
        </div>
        <div id="year">
          <h4>Year</h4>
          <span class="subtitle">{{ header.year }}</span>
        </div>
      </div>
      <div class="location-row" id="vessel-captian-nhp">
        <div id="vessel">
          <h4>Vessel</h4>
          <span class="subtitle">{{ header.vessel }}</span>
        </div>
        <div id="captian">
          <h4>Captian</h4>
          <span class="subtitle">{{ header.captian }}</span>
        </div>
        <div id="nhp">
          <h4>NHP</h4>
          <span class="subtitle">{{ header.nhp }}</span>
        </div>
      </div>
    </div>
  </header>
</template>

<script>


export default {
  components: {
  },
  props: {
    header: {
      type: Object,
      required: true,
    }
  }
}
</script>

<style lang="scss" scoped>
</style>