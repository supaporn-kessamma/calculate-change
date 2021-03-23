<template>
  <div>
    <div class="container mx-auto">
      <button
        class="bg-green-600 px-5 py-3 my-5 text-white rounded-lg"
        @click="
          () => {
            titleName = 'Add Ticket'
            showModal = true
            clear()
          }
        "
      >
        ซื้อตั๋ว
      </button>

      <Modal
        :show="showModal"
        @close="showModal = false"
        :name-header="titleName"
        name-button="บันทึกข้อมูล"
        @apply="applyTicket()"
        class="flex justify-between"
      >
        <div class="mx-4" v-if="titleName === 'Add Ticket'">
          <p class="text-xl inline-block">สถานีต้นทาง</p>
          <select v-model="value">
            <option
              v-for="(departureStation, id) in departureStations"
              :key="id"
              :value="departureStation"
            >
              {{ departureStation }}
            </option>
          </select>
          <p class="text-xl">
            จำนวนผู้โดยสาร <input type="number" v-model="people" />
          </p>
          <div class="">
            <table class="table-fixed">
              <thead>
                <tr>
                  <th class="text-left text-xl">สถานีปลายทาง</th>
                </tr>
              </thead>
              <tbody class="grid grid-cols-3 gap-4">
                <tr
                  v-for="(station, idx) in stations"
                  :station1="station"
                  :key="idx"
                >
                  <td @click="peoplePrice(people, idx)">
                    <input
                      type="radio"
                      v-model="station1"
                      :value="station.name"
                    />
                  </td>
                  <td>{{ station.name }}</td>
                </tr>
              </tbody>
            </table>
          </div>

          <p class="inline-block">
            ยอดรวมทั้งหมด <input type="number" v-model="stationPrice" />
          </p>
          <p class="inline-block">
            ยอดเงินที่รับมา <input type="number" v-model="num2" />
          </p>
          <button
            class="bg-green-600 p-2 text-white rounded-lg"
            @click="
              () => {
                change(stationPrice, num2)
                balance(stationPrice, num2)
              }
            "
          >
            ยืนยันการซื้อ
          </button>
          <p class="text-xl">รับเงินทอน {{ result }}</p>
        </div>

        <div class="mx-4 mt-5" v-if="titleName === 'Show Story'">
          <table class="table-fixed w-full">
            <thead>
              <tr>
                <th>1 บาท</th>
                <th>2 บาท</th>
                <th>5 บาท</th>
                <th>10 บาท</th>
                <th>20 บาท</th>
                <th>50 บาท</th>
                <th>100 บาท</th>
                <th>500 บาท</th>
                <th>1000 บาท</th>
                <th>1000 บาท</th>
              </tr>
            </thead>
            <tbody class="text-center">
              <tr>
                <td>{{ story.one }}</td>
                <td>{{ story.two }}</td>
                <td>{{ story.five }}</td>
                <td>{{ story.ten }}</td>
                <td>{{ story.twenty }}</td>
                <td>{{ story.fifty }}</td>
                <td>{{ story.oneHundred }}</td>
                <td>{{ story.fiveHundred }}</td>
                <td>{{ story.oneThousand }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </Modal>

      <div>
        <p class="text-2xl mb-5">ตารางบันทึกข้อมูลการซื้อตั๋ว</p>
        <table class="table-fixed w-full">
          <thead>
            <tr>
              <th>ต้นทาง</th>
              <th>ปลายทาง</th>
              <th>ยอดรวมทั้งสิ้น</th>
              <th>ยอดเงินที่รับ</th>
              <th>วันที่</th>
            </tr>
          </thead>
          <tbody class="text-center">
            <tr v-for="(story, idx) in stories" :station1="story" :key="idx">
              <td>{{ story.departure }}</td>
              <td>{{ story.station }}</td>
              <td>{{ story.stationPrice }}</td>
              <td>{{ story.num2 }}</td>
              <td>{{ story.dateTime }}</td>
              <td>
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  class="h-8 w-8"
                  @click="show(idx)"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"
                  />
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"
                  />
                </svg>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- <button @click="money()">คำนวณ</button> -->
    <!-- <p>1 บาท {{ one }} เหรียญ</p>
    <p>2 บาท {{ two }} เหรียญ</p>
    <p>5 บาท {{ five }} เหรียญ</p>
    <p>10 บาท {{ ten }} เหรียญ</p>
    <p>20 บาท {{ twenty }} เหรียญ</p>
    <p>50 บาท {{ fifty }} เหรียญ</p>
    <p>100 บาท {{ oneHundred }} เหรียญ</p>
    <p>500 บาท {{ fiveHundred }} เหรียญ</p>
    <p>1000 บาท {{ oneThousand }} เหรียญ</p> -->
  </div>
</template>

<script>
import Modal from '@/components/Modal'
export default {
  components: { Modal },

  data() {
    return {
      showModal: false,
      titleName: '',

      result: 0,
      amount: 0,
      people: 1,
      num2: 0,
      stationPrice: '',
      value: null,
      station1: '',
      one: 0,
      two: 0,
      five: 0,
      ten: 0,
      twenty: 0,
      fifty: 0,
      oneHundred: 0,
      fiveHundred: 0,
      oneThousand: 0,
      dateToDay: new Date().toISOString().slice(0, 10),
      stations: [
        {
          id: 1,
          name: 'บางหว้า',
          price: 40,
        },
        {
          id: 2,
          name: 'บางแค',
          price: 42,
        },
        {
          id: 3,
          name: 'ท่าพระ',
          price: 48,
        },
        {
          id: 4,
          name: 'พระโขนง',
          price: 52,
        },
        {
          id: 5,
          name: 'สยาม',
          price: 59,
        },
        {
          id: 6,
          name: 'มะกะสัน',
          price: 52,
        },
      ],
      departureStations: [
        'บางหว้า',
        'บางแค',
        'ท่าพระ',
        'พระโขนง',
        'สยาม',
        'มะกะสัน',
      ],
      stories: [],
      story: {
        departure: '',
        station: '',
        stationPrice: 0,
        num2: 0,
        dateTime: '',
        one: 0,
        two: 0,
        five: 0,
        ten: 0,
        twenty: 0,
        fifty: 0,
        oneHundred: 0,
        fiveHundred: 0,
        oneThousand: 0,
      },
    }
  },
  // watch: {
  //   people(val) {
  //     this.peoplePrice()
  //   },
  // },
  methods: {
    change(num1, num2) {
      this.result = num2 - num1
      while (this.result >= 1000) {
        this.result -= 1000
        this.oneThousand += 1
      }
      while (this.result >= 500) {
        this.result -= 500
        this.fiveHundred += 1
      }
      while (this.result >= 100) {
        this.result -= 100
        this.oneHundred += 1
      }
      while (this.result >= 50) {
        this.result -= 50
        this.fifty += 1
      }
      while (this.result >= 20) {
        this.result -= 20
        this.twenty += 1
      }
      while (this.result >= 10) {
        this.result -= 10
        this.ten += 1
      }
      while (this.result >= 5) {
        this.result -= 5
        this.five += 1
      }
      while (this.result >= 2) {
        this.result -= 2
        this.two += 1
      }
      while (this.result >= 1) {
        this.result -= 1
        this.one += 1
      }
    },
    peoplePrice(people, idx) {
      this.stationPrice = this.stations[idx].price
      this.stationPrice *= people
    },
    balance(num1, num2) {
      this.result = num2 - num1
    },
    applyTicket() {
      // console.log(this.station1)
      if (this.titleName === 'Add Ticket') {
        this.stories.push({
          departure: this.value,
          station: this.station1,
          stationPrice: this.stationPrice,
          num2: this.num2,
          dateTime: this.dateToDay,
          one: this.one,
          two: this.two,
          five: this.five,
          ten: this.ten,
          twenty: this.twenty,
          fifty: this.fifty,
          oneHundred: this.oneHundred,
          fiveHundred: this.fiveHundred,
          oneThousand: this.oneThousand,
        })

        this.clear()
        this.showModal = false
        // while (this.i < this.result) {
        //   this.text += this.i + 1
        //   this.i++
        // }
      } else if (this.titleName === 'Show Story') {
        this.stories[this.nowShow] = this.story
        this.showModal = false
        this.clear()
      }
    },
    show(idx) {
      this.story.one = this.stories[idx].one
      this.story.two = this.stories[idx].two
      this.story.five = this.stories[idx].five
      this.story.ten = this.stories[idx].ten
      this.story.twenty = this.stories[idx].twenty
      this.story.fifty = this.stories[idx].fifty
      this.story.oneHundred = this.stories[idx].oneHundred
      this.story.fiveHundred = this.stories[idx].fiveHundred
      this.story.oneThousand = this.stories[idx].oneThousand

      this.titleName = 'Show Story'
      this.nowShow = idx
      this.showModal = true
    },
    clear() {
      this.story = {}
      this.result = 0
      this.amount = 0
      this.people = 1
      this.num2 = 0
      this.stationPrice = ''
      this.value = null
      this.station1 = ''
      this.one = 0
      this.two = 0
      this.five = 0
      this.ten = 0
      this.twenty = 0
      this.fifty = 0
      this.oneHundred = 0
      this.fiveHundred = 0
      this.oneThousand = 0
    },
    // money() {
    //   while (this.i < this.result) {
    //     this.text += this.i + 1
    //     this.i++
    //   }
    // },
  },
}
</script>

<style></style>
