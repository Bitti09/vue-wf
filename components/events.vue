/* eslint-disable vue/no-use-v-if-with-v-for */
<template>
  <div class="w-full h-auto border-transparent rounded">
    <div
      class="px-4 pt-4 text-4xl font-bold bg-transparent text-primary sm:px-6"
    >
      Events
    </div>
    <div class="pt-0 bg-box text-primary">
      <div v-if="!events" class="text-primary">
        <div
          class="relative px-4 py-3 text-red-700 bg-transparent border border-red-400 rounded"
          role="alert"
        >
          <strong class="font-bold">No Events today</strong>
        </div>
      </div>
      <template v-else>
        <badger-accordion ref="myAccordion" :icons="false">
          <badger-accordion-item v-for="event in events" :key="event.id">
            <div slot="header" class="px-2">
              <div
                class="grid items-center h-10 grid-cols-4 py-1 bg-grey-light"
              >
                <div class="col-span-3">{{ event.Name }}</div>
                <div class="col-span-1">{{ event.HealthPct.toFixed(2) }} %</div>
              </div>
            </div>
            <div
              slot="content"
              class="break-words border-b border-gray-600 bg-box"
            >
              <div
                class="grid items-center h-10 grid-cols-4 px-2 py-1 bg-grey-light"
              >
                <div class="col-span-2 xl:col-span-3">Ends in</div>
                <div class="col-span-2 xl:col-span-1">
                  {{ timediff(event.Ends) }}
                </div>
              </div>
              <div
                v-if="event.Mainreward"
                class="grid items-center h-10 grid-cols-4 px-2 py-1 bg-grey-light"
              >
                <div class="col-span-2 xl:col-span-3">Main Reward</div>
                <div class="col-span-2 xl:col-span-1">
                  {{ event.Mainreward }}
                </div>
              </div>
              <div
                v-if="event.Mainrewardcredits > 0"
                class="grid items-center h-10 grid-cols-4 px-2 py-1 bg-grey-light"
              >
                <div class="col-span-2 xl:col-span-3">Credits Reward</div>
                <div class="col-span-2 xl:col-span-1">
                  {{ event.Mainrewardcredits }}
                </div>
              </div>
              <div
                v-if="event.Mainrewardxp > 0"
                class="grid items-center h-10 grid-cols-4 px-2 py-1 bg-grey-light"
              >
                <div class="col-span-2 xl:col-span-3">Reward XP</div>
                <div class="col-span-2 xl:col-span-1">
                  {{ event.Mainrewardxp }}
                </div>
              </div>
              <div v-if="event.InterimRewards" class="px-2 py-1 bg-grey-light">
                Personal Interim Rewards:
                <div
                  v-for="(reward, indexr) in event.InterimRewards"
                  :key="indexr"
                  class="grid items-center h-10 grid-cols-7 py-1 border-b-4 border-gray-600 bg-grey-light"
                >
                  <div class="col-span-1">
                    at {{ event.InterimGoalsteps[indexr] }}
                  </div>
                  <div class="col-span-4 xl:col-span-3">{{ reward.Item }}</div>
                  <div class="col-span-2 xl:col-span-3">
                    + {{ reward.Credits }}C & {{ reward.XP }}XP
                  </div>
                </div>
              </div>
              <div v-if="event.Jobs" class="px-2 py-1 bg-grey-light">
                Event Jobs:
                <badger-accordion ref="myAccordion2" :icons="false">
                  <badger-accordion-item
                    v-for="(job, indexr) in event.Jobs"
                    :key="indexr"
                  >
                    <div slot="header" class="">
                      <div
                        class="grid items-center h-full grid-cols-2 py-1 bg-grey-light"
                      >
                        <div class="col-span-1 text-left">
                          {{ job.Type }}
                        </div>
                        <div class="col-span-1 text-left">
                          Level: {{ job.Minenemylvl }}-{{ job.Maxenemylvl }}
                        </div>
                      </div>
                    </div>
                    <div
                      slot="content"
                      class="break-words border-b-4 lg:break-none bg-box"
                    >
                      <div
                        class="grid items-center h-full grid-cols-2 py-1 bg-grey-light"
                      >
                        <div class="w-full h-10 py-2 text-center border-b">
                          {{ $tc('tracker.rewards', 2) }}
                        </div>
                        <div class="w-full h-10 py-2 text-center border-b">
                          {{ $t('tracker.standing') }}
                        </div>

                        <div
                          class="content-center w-full col-span-1 py-1 text-left h-30"
                        >
                          <span
                            v-for="(reward1, index) in job.ItemRewards"
                            :key="index"
                            :index="index"
                          >
                            {{ reward1 }},<br />
                          </span>
                        </div>
                        <div
                          class="content-center w-full col-span-1 py-1 text-left"
                        >
                          <p
                            v-for="(stand, index) in job.XPreward"
                            :key="index"
                            :index="index"
                          >
                            {{ stand }}<br />
                          </p>
                        </div>
                      </div></div
                  ></badger-accordion-item>
                </badger-accordion>
              </div>
            </div>
          </badger-accordion-item> </badger-accordion
        ><!----->
      </template>
    </div>
  </div>
</template>

<style scoped>
.badger-accordion__header.js-badger-accordion-head.badger-accordion-togg.badger-accordion-title {
  flex: 0 0 100%;
}
</style>

<script>
import moment from 'moment'
export default {
  name: 'InvasionsPanel',
  // eslint-disable-next-line
  props: ['events'],
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
      const now = moment(new Date()) // todays date
      // eslint-disable-next-line
      var ms = moment.duration(start.diff(now))
      // eslint-disable-next-line
      let days = ''
      let hours = ''
      let minutes = ' '
      let seconds = ' '
      days = ms.days().toFixed(0)
      hours = ms.hours().toFixed(0)
      minutes = ms.minutes()
      seconds = ms.seconds()
      const t = days + 'd ' + hours + 'h ' + minutes + 'm ' + seconds + 's'
      return t
    }
  }
}
</script>
