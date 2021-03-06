<template lang="pug">
base-notification(
  :can-remove="false",
  :notification="{}",
  :read-after-click="false",
  @click="action"
)
  .background(slot="content")
    .text
      .title {{ $t('worldBoss') }}
      .sub-title {{ $t('questDysheartenerText') }}
    .d-flex.align-items-center.justify-content-left
      div
        .left-hearts
      .float-right
        .quest_dysheartener_notification
        .phobia_dysheartener_notification
    .health-bar.d-flex.align-items-center.justify-content-center
      .svg-icon(v-html="icons.health")
      .boss-health-wrap
        .boss-health-bar(:style="{width: (parseInt(bossHp) / questData.boss.hp) * 100 + '%'}")
      .pending-damage
        .svg-icon(v-html="icons.sword")
        span +{{parseInt(user.party.quest.progress.up) || 0}}
</template>

<style lang="scss" scoped>
  .background {
    position: relative;
  }

  .notification {
    background-image: linear-gradient(to right, #410f2a, #931f4d 50%, #410f2a) !important;
    padding: 0 !important;
  }

  .notification /deep/ .notification-content {
    margin-top: 0 !important;
    margin-bottom: 0 !important;
  }

  .title, .sub-title {
    color: #fff;
  }

  .title {
    font-size: 14px;
    font-weight: bold;
  }

  .text {
    position: absolute;
    margin-top: 1em;
    margin-left: 1em;
  }

  .sub-title {
    font-size: 14px;
  }

  .left-hearts {
    background-size: 100%;
    margin-left: 2em;
    margin-right: 1em;
    width: 106px;
    height: 41px;
    background-size: contain;
  }

  .left-hearts {
    background-image: url('~client/assets/images/world-boss/left-hearts@3x.png');
  }

  .quest_dysheartener_notification, .phobia_dysheartener_notification {
    width: 219px;
    height: 64px;
    background-size: 100%;
  }

  .phobia_dysheartener_notification {
    display: none;
  }

  .quest_dysheartener_notification {
    background-image: url('~client/assets/images/world-boss/mantis-static-notification@3x.png');
  }

  .phobia_dysheartener_notification {
    display: none;
    background-image: url('~client/assets/images/world-boss/heart-translucent-shadow-notification@3x.png');
  }

  .health-bar {
    width: 378px;
    height: 24px;
    background-color: #410f2a;
  }

  .boss-health-wrap {
    margin-left: .5em;
    margin-right: .5em;
    width: 274px;
    height: 12px;
    background-color: rgba(255, 255, 255, 0.24);
    border-radius: 2px;
  }

  .boss-health-bar {
    background-color: #f74e52;
    height: 12px;
    margin-bottom: .5em;
    border-radius: 2px;
  }

  .svg-icon {
    width: 16px;
    height: 16px;
  }

  .pending-damage {
    .svg-icon {
      display: inline-block;
      margin-right: .2em;
      margin-bottom: .2em;
    }

    span, .svg-icon {
      vertical-align: bottom;
    }

    color: #fff;
  }
</style>

<script>
import quests from 'common/script/content/quests';

import { mapState } from 'client/libs/store';
import BaseNotification from './base';

import health from 'assets/svg/health.svg';
import sword from 'assets/svg/sword.svg';

export default {
  components: {
    BaseNotification,
  },
  data () {
    const questData = quests.quests.dysheartener;

    return {
      icons: Object.freeze({
        health,
        sword,
      }),
      questData,
      worldBoss: {},
    };
  },
  async mounted () {
    const result = await this.$store.dispatch('worldState:getWorldState');
    this.worldBoss = result.worldBoss;
  },
  computed: {
    ...mapState({user: 'user.data'}),
    bossHp () {
      if (this.worldBoss && this.worldBoss.progress) {
        return this.worldBoss.progress.hp;
      }
      return this.questData.boss.hp.toLocaleString();
    },
  },
  methods: {
    action () {
      this.$router.push({name: 'tavern'});
    },
  },
};
</script>
