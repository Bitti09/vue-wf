<template>
  <div
    class="h-auto max-w-lg overflow-hidden border-transparent rounded md:max-w-md sm:max-w-sm xl:max-w-xl"
  >
    <div
      class="px-4 pt-4 text-4xl font-bold bg-transparent text-primary sm:px-6"
    >
      {{ $tc('tracker.invasions', 2) }}
    </div>
    <div class="pt-0 bg-box text-primary">
      <div v-if="invasions.length < 1" class="text-primary">
        <div
          class="relative px-4 py-3 text-red-700 bg-transparent border border-red-400 rounded"
          role="alert"
        >
          <strong class="font-bold">No Invasions today</strong>
        </div>
      </div>
      <template v-else>
        <div
          class="grid items-center grid-cols-2 px-2 py-2 pl-2 pr-4 border-b-4 border-gray-600"
        >
          <div class="w-full h-auto">Factions</div>
          <span class="w-full h-auto text-right pr-9 number-col">
            Location
          </span>
        </div>
        <badger-accordion ref="myAccordion" :icons="false">
          <badger-accordion-item v-for="inva in invasions" :key="inva.id">
            <div slot="header" class="flex items-center px-2">
              <p class="w-full">
                <span :class="getcolourfaction(inva.AttackerMissionInfo)">
                  {{ inva.AttackerMissionInfo }}</span
                >
                vs
                <span :class="getcolourfaction(inva.DefenderMissionInfo)">{{
                  inva.DefenderMissionInfo
                }}</span>
              </p>
              <span class="w-3/4 p-2 pr-0 text-right">{{ inva.Location }}</span>
            </div>
            <div
              slot="content"
              class="break-all border-b border-gray-600 bg-box"
            >
              <div class="grid grid-cols-2 gap-0 px-2 py-1">
                <div class="w-full">
                  {{ $t('tracker.complete') }}
                </div>
                <div class="w-full">
                  {{ parseFloat(complete(inva.Completion)).toFixed(2) }} %
                  {{ $t('tracker.complete2') }}
                </div>
                <div class="w-full">
                  {{ $t('tracker.missiontype') }}
                </div>
                <div class="w-full break-normal">{{ inva.MissionType }}</div>
                <div v-if="inva.AttackerRewardItem" class="w-full">
                  {{ $t('tracker.attacker') }} {{ $tc('tracker.rewards', 1) }}:
                </div>
                <div v-if="inva.AttackerRewardItem" class="w-full break-normal">
                  {{ inva.AttackerRewardItem }}
                </div>
                <div v-if="inva.DefenderRewardItem" class="w-full">
                  {{ $t('tracker.defender') }} {{ $tc('tracker.rewards', 1) }}:
                </div>
                <div v-if="inva.DefenderRewardItem" class="w-full break-normal">
                  {{ inva.DefenderRewardItem }}
                </div>
              </div>
            </div>
          </badger-accordion-item>
        </badger-accordion>
      </template>
    </div>
  </div>
</template>

<style scoped>
table.table div,
table.table th,
table.table > thead > tr {
  border: 0;
}
</style>

<script>
import moment from 'moment'
export default {
  name: 'InvasionsPanel',
  // eslint-disable-next-line
  props: ['invasions'],
  data() {
    return {
      platinum: 'platinum'
    }
  },
  computed: {
    headertext() {
      return 'Invasions'
    },
    activeplat() {
      return this.$store.state.activeplatform.short
    },
    activelang1() {
      return this.$store.state.activelang.short
    }
  },
  methods: {
    getcolourfaction(prop) {
      let x = null
      if (prop === 'Grineer') {
        x = 'text-red-600'
      } else if (prop === 'Corpus') {
        x = 'text-blue-600'
      } else if (prop === 'Infested') {
        x = 'text-green-600'
      } else {
        x = 'text-white'
      }
      return x
    },
    complete(prop) {
      let x = prop
      if (prop < 0) {
        x = x * -1
      }
      return x
    },
    timediff(var1) {
      var1 = var1 * 1
      const start = moment(var1)
      // eslint-disable-next-line
      var ms = moment.duration(start.diff(moment().valueOf()))
      // eslint-disable-next-line
      let days = ''
      let hours = ''
      let minutes = ' '
      let seconds = ' '
      if (ms.days() * -1 > 0) {
        days = ms.days() * -1 + 'd '
      }
      if (ms.hours() > 0) {
        hours = ms.hours() + 'h:'
      }
      if (ms.minutes() > 0) {
        minutes = ms.minutes() + 'm:'
      }
      if (ms.seconds() > 0) {
        seconds = ms.seconds() + 's'
      }
      const t = days + hours + minutes + seconds
      return t
    }
  }
}
</script>
