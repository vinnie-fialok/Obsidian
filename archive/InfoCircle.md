```html
<template>
  <div class="alert-badge">
    <div class="badge__wrapper">
      <slot />
      <wtg-tooltip bottom>
        <template #activator="{ on }">
          <div class="alert-badge--info badge__content" v-on="on">
            <wtg-icon>s-icon-info-circle</wtg-icon>
          </div>
        </template>
        <span>{{ tooltip }}</span>
      </wtg-tooltip>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { WtgIcon, WtgTooltip } from "wtg-material-ui";

export default defineComponent({
  name: "InfoBadge",
  components: {
    WtgIcon,
    WtgTooltip,
  },
  props: {
    tooltip: {
      type: String,
      required: true,
    },
  },
});
</script>
<style scoped lang="scss">
.alert-badge {
  display: inline-flex;
  line-height: 1;

  .badge__wrapper {
    display: flex;
    position: relative;
  }

  .badge__content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: var(--s-padding-xs);
    border-radius: var(--s-radius-xxl);
    position: absolute;
    float: right;
    z-index: 1;
    right: calc(-1 * var(--s-sizing-m) / 2);
    top: calc(-1 * var(--s-sizing-m) / 2);
    height: var(--s-sizing-m);
    width: var(--s-sizing-m);

    .wtg-icon {
      height: var(--s-sizing-m);
      width: var(--s-sizing-m);
      font-size: var(--s-sizing-m);
    }
  }

  .alert-badge--info {
    color: var(--s-info-icon-default);
  }
}
</style>

```