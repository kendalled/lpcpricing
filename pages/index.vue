<template>
  <div id="pageroot" class="bg-gray-100 text-center mx-auto pt-24">
    <!-- Full width column for heading -->
    <div class="flex mb-4">
      <h2 class="flex-1 py-2 text-gray-800 text-3xl">
        Quote Calculator for Lapel Pins
      </h2>
    </div>

    <!-- Three columns -->
    <div class="flex mb-4 px-12">
      <div class="flex-1 mx-6">
        <label class="typo__label">Select Style:</label>
        <multiselect
          v-model="pinType"
          :options="styles"
          placeholder="Select one"
          label="name"
          track-by="name"
          :allow-empty="false"
          :show-labels="false"
          :block-keys="['Tab', 'Enter']"
        />
        <br>
        <label class="typo__label">Select Sizing:</label>
        <multiselect
          v-model="pinSizing"
          :options="sizingOptions"
          placeholder="Select one"
          label="name"
          track-by="name"
          :allow-empty="false"
          :show-labels="false"
          :block-keys="['Tab', 'Enter']"
        />
        <br>
        <label class="typo__label">Select Quantity:</label>
        <multiselect
          v-model="pinQty"
          :options="quantityOptions"
          placeholder="Select one"
          label="name"
          track-by="name"
          :allow-empty="false"
          :show-labels="false"
          :block-keys="['Tab', 'Enter']"
        />
        <br>
        <label class="typo__label">Select Plating:</label>
        <multiselect
          v-model="pinPlating"
          :options="platingOptions"
          :custom-label="concatKeys"
          placeholder="Select one"
          label="name"
          track-by="name"
          :allow-empty="false"
          :show-labels="false"
          :block-keys="['Tab', 'Enter']"
        />
      </div>
      <div class="flex-1 mx-6">
        <label class="typo__label">Select Backing:</label>
        <multiselect
          v-model="pinBacking"
          :options="backingOptions"
          :custom-label="concatKeys"
          placeholder="Select one"
          label="name"
          track-by="name"
          :allow-empty="false"
          :show-labels="false"
          :block-keys="['Tab', 'Enter']"
        />
        <br>
        <label class="typo__label">Select Packaging:</label>
        <multiselect
          v-model="pinPackaging"
          :options="packagingOptions"
          :custom-label="concatKeys"
          placeholder="Select one"
          label="name"
          track-by="name"
          :allow-empty="false"
          :show-labels="false"
          :block-keys="['Tab', 'Enter']"
        />
        <br>
        <label class="typo__label">Select Add-Ons:</label>
        <multiselect
          v-model="pinAddons"
          :options="addonOptions"
          :multiple="true"
          :close-on-select="false"
          :clear-on-select="false"
          :preserve-search="true"
          placeholder="Select Add-Ons"
          :custom-label="concatKeys"
          label="name"
          track-by="name"
          :allow-empty="!false"
          :preselect-first="false"
        >
          <template slot="selection" slot-scope="{ values, search, isOpen }">
            <span v-if="values.length &amp;&amp; !isOpen" class="multiselect__single">{{ values.length }} add-ons selected</span>
          </template>
        </multiselect>
        <br>
        <label class="typo__label">Select Discount:</label>
        <multiselect
          v-model="pinDiscount"
          :options="discountOptions"
          :custom-label="concatKeys"
          placeholder="Select one"
          label="name"
          track-by="name"
          :show-labels="false"
          :allow-empty="false"
          :block-keys="['Tab', 'Enter']"
        />
        <div class="flex pt-6">
          <div class="w-1/2">
            <Counter v-if="pinAddons.indexOf(addonOptions[0]) !== -1" label-text="Glitter Color(s)" @increment="handleChange(1, 0)" @decrement="handleChange(0, 0)" />
            <Counter v-if="pinAddons.indexOf(addonOptions[1]) !== -1" label-text="Gemstone(s)" @increment="handleChange(1, 1)" @decrement="handleChange(0, 1)" />
            <Counter v-if="pinAddons.indexOf(addonOptions[2]) !== -1" label-text="Cutout(s)" @increment="handleChange(1, 2)" @decrement="handleChange(0, 2)" />
          </div>
          <div class="w-1/2">
            <Counter v-if="pinAddons.indexOf(addonOptions[3]) !== -1" label-text="Extra Post(s)" @increment="handleChange(1, 3)" @decrement="handleChange(0, 3)" />
            <Counter v-if="pinAddons.indexOf(addonOptions[4]) !== -1" label-text="Epoxy Dome(s)" @increment="handleChange(1, 4)" @decrement="handleChange(0, 4)" />
          </div>
        </div>
      </div>
      <div class="flex-1 mx-6">
        <h2 class="text-xl h-10 bg-white border-b-2 border-blue-500 shadow rounded my-6 text-gray-800 leading-loose tracking-tight">
          Per Unit Price: <span class="text-green-600">${{ result }}</span>
        </h2>
        <br>
        <h2 class="text-xl h-10 bg-white border-b-2 border-gray-500 shadow rounded mb-6 text-gray-800 leading-loose tracking-tight">
          Retail Price: <span class="text-gray-600">${{ Math.round((result * pinQty.val + staticExtras) * 100) / 100 }}</span>
        </h2>
        <br>
        <h2 class="text-xl h-10 bg-white border-b-2 border-orange-500 shadow rounded mb-6 text-gray-800 leading-loose tracking-tight">
          Quote Price: <span class="text-green-600">${{ Math.round(((result * pinQty.val + staticExtras) * pinDiscount.val) * 100) / 100 }}</span>
        </h2>
        <br>
        <div v-if="!isHidden" class="p-2">
          <div v-if="Math.round((result * pinQty.val + staticExtras) * 100) / 100 == Math.round(((result * pinQty.val + staticExtras) * pinDiscount.val) * 100) / 100" class="inline-flex hover:cursor-pointer hover:bg-gray-100 items-center bg-white leading-none text-pink-600 rounded-full p-2 shadow text-teal text-sm" @click="isHidden = true">
            <span class="inline-flex bg-pink-600 text-white rounded-full h-6 px-3 justify-center items-center">Alert</span>
            <span class="inline-flex px-2">No discount applied - click to dismiss.</span>
          </div>
          <div v-else class="inline-flex hover:cursor-pointer hover:bg-gray-100 items-center bg-white leading-none text-green-600 rounded-full p-2 shadow text-teal text-sm" @click="isHidden = true">
            <span class="inline-flex bg-green-600 text-white rounded-full h-6 px-3 justify-center items-center">Success</span>
            <span class="inline-flex px-2">Discount applied - click to dismiss.</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Multiselect from 'vue-multiselect'
import Counter from '~/components/Counter'
export default {
  head: {
    link: [
      { rel: 'stylesheet', href: 'https://firebasestorage.googleapis.com/v0/b/lpcpricing.appspot.com/o/vue-multiselect.min.css?alt=media&token=3d643c8a-78ba-4d00-ac47-9938f1e679a5' }
    ]
  },
  components: {
    Multiselect,
    Counter
  },
  data () {
    return {
      isHidden: false,
      pinType: { name: 'Style', key: 'style' },
      styles: [
        { name: 'Soft Enamel', key: 'softEnamel' },
        { name: 'Hard Enamel', key: 'hardEnamel' },
        { name: 'Offset Printed', key: 'offsetPrinted' },
        { name: 'Die Struck', key: 'dieStruck' },
        { name: 'Silk Screen', key: 'silkScreen' }
      ],
      pinPlating: { name: 'Plating', cost: '$0.00' },
      platingOptions: [
        { name: 'Polished Gold', cost: '$0.00' },
        { name: 'Polished Silver', cost: '$0.00' },
        { name: 'Polished Copper', cost: '$0.00' },
        { name: 'Polished Nickel', cost: '$0.00' },
        { name: 'Black Metal', cost: '$0.00' },
        { name: 'Antiqued Gold', cost: '$0.40' },
        { name: 'Antiqued Silver', cost: '$0.35' },
        { name: 'Antiqued Copper', cost: '$0.30' },
        { name: 'Dual Plated', cost: '$0.70' },
        { name: 'Color Coated', cost: '$0.25' }
      ],
      pinSizing: { name: 'Size', key: 'threefourths' },
      sizingOptions: [
        { name: '.75\'', key: 'threefourths' },
        { name: '1.0\'', key: 'one' },
        { name: '1.25\'', key: 'onetwofive' },
        { name: '1.50\'', key: 'onefivezero' },
        { name: '1.75\'', key: 'onesevenfive' },
        { name: '2.0\'', key: 'two' }
      ],
      pinBacking: { name: 'Backing', cost: '$0.00' },
      backingOptions: [
        { name: 'Butterfly Clutch', cost: '$0.00' },
        { name: 'Black Rubber', cost: '$0.00' },
        { name: 'Yellow Rubber', cost: '$0.00' },
        { name: 'Deluxe Clutch', cost: '$0.20' },
        { name: 'Safety Pin', cost: '$0.20' },
        { name: 'Jewelry Clutch', cost: '$0.30' },
        { name: 'Keychain Loop', cost: '$0.50' },
        { name: 'Cufflink Clutch', cost: '$0.75' },
        { name: 'Round Magnet', cost: '$0.75' },
        { name: 'Bar Magnet', cost: '$1.50' }
      ],
      pinPackaging: { name: 'Packaging', cost: '$0.00' },
      packagingOptions: [
        { name: 'Poly Bag', cost: '$0.00' },
        { name: 'Velvet Bag', cost: '$0.55' },
        { name: 'Acrylic Case', cost: '$1.00' },
        { name: 'Velvet Case', cost: '$4.00' }
      ],
      pinAddons: [],
      addonOptions: [
        { name: 'Glitter(s)', qty: 1, cost: '$0.25' },
        { name: 'Gemstone(s)', qty: 1, cost: '$0.20' },
        { name: 'Cutout(s)', qty: 1, cost: '$0.10' },
        { name: 'Extra Post(s)', qty: 1, cost: '$0.08' },
        { name: 'Epoxy Dome(s)', qty: 1, cost: '$0.20' },
        { name: 'Keychain Loop', qty: 1, cost: '$0.50' },
        { name: '3D Mold', qty: 1, cost: '$100.00' },
        { name: 'Zinc Alloy Mold', qty: 1, cost: '$30.00' },
        { name: 'Custom Backing', qty: 1, cost: '$30.00' }
      ],
      pinQty: { name: 'Quantity', key: 'onehundred', val: 0 },
      quantityOptions: [
        { name: '100', key: 'onehundred', val: 100 },
        { name: '200', key: 'twohundred', val: 200 },
        { name: '300', key: 'threehundred', val: 300 },
        { name: '500', key: 'fivehundred', val: 500 },
        { name: '750', key: 'sevenfifty', val: 750 },
        { name: '1000', key: 'thousand', val: 1000 },
        { name: '2000', key: 'twothousand', val: 2000 }
      ],
      pinDiscount: { name: 'Discount', val: 1.0, cost: 'Full Price' },
      discountOptions: [
        { name: 'SAMEDAY', val: 0.95, cost: '5% Off' },
        { name: 'FIRSTORDER', val: 0.9, cost: '10% Off' },
        { name: 'REORDER', val: 0.9, cost: '10% Off' },
        { name: 'LPC20', val: 0.8, cost: '20% Off' },
        { name: 'RESELLER', val: 0.75, cost: '25% Off' }
      ]
    }
  },
  computed: {
    result () {
      const style = this.pinType.key
      const sizing = this.pinSizing.key
      const qty = this.pinQty.key
      const pricing = {
        style: {
          threefourths: {
            onehundred: '$0.00',
            twohundred: '$0.00',
            threehundred: '$0.00',
            fivehundred: '$0.00',
            sevenfifty: '$0.00',
            thousand: '$0.00',
            twothousand: '$0.00'
          },
          one: {
            onehundred: '$0.00',
            twohundred: '$0.00',
            threehundred: '$0.00',
            fivehundred: '$0.00',
            sevenfifty: '$0.00',
            thousand: '$0.00',
            twothousand: '$0.00'
          },
          onetwofive: {
            onehundred: '$0.00',
            twohundred: '$0.00',
            threehundred: '$0.00',
            fivehundred: '$0.00',
            sevenfifty: '$0.00',
            thousand: '$0.00',
            twothousand: '$0.00'
          },
          onefivezero: {
            onehundred: '$0.00',
            twohundred: '$0.00',
            threehundred: '$0.00',
            fivehundred: '$0.00',
            sevenfifty: '$0.00',
            thousand: '$0.00',
            twothousand: '$0.00'
          },
          onesevenfive: {
            onehundred: '$0.00',
            twohundred: '$0.00',
            threehundred: '$0.00',
            fivehundred: '$0.00',
            sevenfifty: '$0.00',
            thousand: '$0.00',
            twothousand: '$0.00'
          },
          two: {
            onehundred: '$0.00',
            twohundred: '$0.00',
            threehundred: '$0.00',
            fivehundred: '$0.00',
            sevenfifty: '$0.00',
            thousand: '$0.00',
            twothousand: '$0.00'
          }
        },
        softEnamel: {
          threefourths: {
            onehundred: '$2.55',
            twohundred: '$2.01',
            threehundred: '$1.47',
            fivehundred: '$1.23',
            sevenfifty: '$1.12',
            thousand: '$1.07',
            twothousand: '$0.96'
          },
          one: {
            onehundred: '$2.62',
            twohundred: '$2.09',
            threehundred: '$1.55',
            fivehundred: '$1.27',
            sevenfifty: '$1.17',
            thousand: '$1.10',
            twothousand: '$0.98'
          },
          onetwofive: {
            onehundred: '$2.69',
            twohundred: '$2.23',
            threehundred: '$1.66',
            fivehundred: '$1.33',
            sevenfifty: '$1.21',
            thousand: '$1.13',
            twothousand: '$1.04'
          },
          onefivezero: {
            onehundred: '$2.86',
            twohundred: '$2.43',
            threehundred: '$1.95',
            fivehundred: '$1.44',
            sevenfifty: '$1.31',
            thousand: '$1.24',
            twothousand: '$1.11'
          },
          onesevenfive: {
            onehundred: '$3.18',
            twohundred: '$2.68',
            threehundred: '$2.20',
            fivehundred: '$1.71',
            sevenfifty: '$1.57',
            thousand: '$1.51',
            twothousand: '$1.41'
          },
          two: {
            onehundred: '$3.38',
            twohundred: '$2.89',
            threehundred: '$2.40',
            fivehundred: '$1.90',
            sevenfifty: '$1.77',
            thousand: '$1.69',
            twothousand: '$1.59'
          }
        },
        offsetPrinted: {
          threefourths: {
            onehundred: '$2.78',
            twohundred: '$2.20',
            threehundred: '$1.62',
            fivehundred: '$1.38',
            sevenfifty: '$1.30',
            thousand: '$1.24',
            twothousand: '$1.12'
          },
          one: {
            onehundred: '$2.82',
            twohundred: '$2.27',
            threehundred: '$1.72',
            fivehundred: '$1.43',
            sevenfifty: '$1.34',
            thousand: '$1.27',
            twothousand: '$1.14'
          },
          onetwofive: {
            onehundred: '$2.89',
            twohundred: '$2.35',
            threehundred: '$1.82',
            fivehundred: '$1.49',
            sevenfifty: '$1.40',
            thousand: '$1.31',
            twothousand: '$1.19'
          },
          onefivezero: {
            onehundred: '$3.16',
            twohundred: '$2.59',
            threehundred: '$2.12',
            fivehundred: '$1.59',
            sevenfifty: '$1.49',
            thousand: '$1.41',
            twothousand: '$1.27'
          },
          onesevenfive: {
            onehundred: '$3.35',
            twohundred: '$2.70',
            threehundred: '$2.43',
            fivehundred: '$1.93',
            sevenfifty: '$1.83',
            thousand: '$1.75',
            twothousand: '$1.69'
          },
          two: {
            onehundred: '$3.61',
            twohundred: '$2.99',
            threehundred: '$2.64',
            fivehundred: '$2.19',
            sevenfifty: '$2.11',
            thousand: '$1.99',
            twothousand: '$1.95'
          }
        },
        dieStruck: {
          threefourths: {
            onehundred: '$2.54',
            twohundred: '$2.00',
            threehundred: '$1.46',
            fivehundred: '$1.22',
            sevenfifty: '$1.11',
            thousand: '$1.06',
            twothousand: '$0.95'
          },
          one: {
            onehundred: '$2.61',
            twohundred: '$2.08',
            threehundred: '$1.54',
            fivehundred: '$1.26',
            sevenfifty: '$1.16',
            thousand: '$1.09',
            twothousand: '$0.97'
          },
          onetwofive: {
            onehundred: '$2.68',
            twohundred: '$2.22',
            threehundred: '$1.65',
            fivehundred: '$1.32',
            sevenfifty: '$1.20',
            thousand: '$1.12',
            twothousand: '$1.03'
          },
          onefivezero: {
            onehundred: '$2.85',
            twohundred: '$2.43',
            threehundred: '$1.94',
            fivehundred: '$1.43',
            sevenfifty: '$1.30',
            thousand: '$1.23',
            twothousand: '$1.10'
          },
          onesevenfive: {
            onehundred: '$3.17',
            twohundred: '$2.67',
            threehundred: '$2.19',
            fivehundred: '$1.70',
            sevenfifty: '$1.56',
            thousand: '$1.50',
            twothousand: '$1.40'
          },
          two: {
            onehundred: '$3.37',
            twohundred: '$2.88',
            threehundred: '$2.39',
            fivehundred: '$1.89',
            sevenfifty: '$1.76',
            thousand: '$1.68',
            twothousand: '$1.58'
          }
        },
        hardEnamel: {
          threefourths: {
            onehundred: '$2.66',
            twohundred: '$2.11',
            threehundred: '$1.55',
            fivehundred: '$1.28',
            sevenfifty: '$1.19',
            thousand: '$1.11',
            twothousand: '$1.04'
          },
          one: {
            onehundred: '$2.69',
            twohundred: '$2.17',
            threehundred: '$1.66',
            fivehundred: '$1.36',
            sevenfifty: '$1.23',
            thousand: '$1.19',
            twothousand: '$1.14'
          },
          onetwofive: {
            onehundred: '$2.80',
            twohundred: '$2.26',
            threehundred: '$1.75',
            fivehundred: '$1.43',
            sevenfifty: '$1.30',
            thousand: '$1.23',
            twothousand: '$1.19'
          },
          onefivezero: {
            onehundred: '$3.05',
            twohundred: '$2.53',
            threehundred: '$2.00',
            fivehundred: '$1.53',
            sevenfifty: '$1.43',
            thousand: '$1.38',
            twothousand: '$1.30'
          },
          onesevenfive: {
            onehundred: '$3.30',
            twohundred: '$2.73',
            threehundred: '$2.33',
            fivehundred: '$1.83',
            sevenfifty: '$1.73',
            thousand: '$1.65',
            twothousand: '$1.59'
          },
          two: {
            onehundred: '$3.57',
            twohundred: '$3.07',
            threehundred: '$2.58',
            fivehundred: '$2.09',
            sevenfifty: '$1.99',
            thousand: '$1.91',
            twothousand: '$1.85'
          }
        },
        silkScreen: {
          threefourths: {
            onehundred: '$2.54',
            twohundred: '$2.00',
            threehundred: '$1.46',
            fivehundred: '$1.22',
            sevenfifty: '$1.11',
            thousand: '$1.06',
            twothousand: '$0.95'
          },
          one: {
            onehundred: '$2.61',
            twohundred: '$2.08',
            threehundred: '$1.54',
            fivehundred: '$1.26',
            sevenfifty: '$1.16',
            thousand: '$1.09',
            twothousand: '$0.97'
          },
          onetwofive: {
            onehundred: '$2.68',
            twohundred: '$2.22',
            threehundred: '$1.65',
            fivehundred: '$1.32',
            sevenfifty: '$1.20',
            thousand: '$1.12',
            twothousand: '$1.03'
          },
          onefivezero: {
            onehundred: '$2.85',
            twohundred: '$2.42',
            threehundred: '$1.94',
            fivehundred: '$1.43',
            sevenfifty: '$1.30',
            thousand: '$1.23',
            twothousand: '$1.10'
          },
          onesevenfive: {
            onehundred: '$3.17',
            twohundred: '$2.67',
            threehundred: '$2.19',
            fivehundred: '$1.70',
            sevenfifty: '$1.56',
            thousand: '$1.50',
            twothousand: '$1.40'
          },
          two: {
            onehundred: '$3.37',
            twohundred: '$2.88',
            threehundred: '$2.39',
            fivehundred: '$1.89',
            sevenfifty: '$1.76',
            thousand: '$1.68',
            twothousand: '$1.58'
          }
        }
      }
      // get values of extras
      const plating = parseFloat(this.pinPlating.cost.replace('$', ''))
      const backing = parseFloat(this.pinBacking.cost.replace('$', ''))
      const packaging = parseFloat(this.pinPackaging.cost.replace('$', ''))
      let addons = 0.0
      for (let i = 0; i < this.pinAddons.length; i++) {
        if (this.pinAddons[i].name !== '3D Mold' && this.pinAddons[i].name !== 'Zinc Alloy Mold' && this.pinAddons[i].name !== 'Custom Backing') {
          addons += (parseFloat(this.pinAddons[i].cost.replace('$', '')) * this.pinAddons[i].qty)
        }
      }
      // cost of all extras
      const extraCost = plating + backing + packaging + addons
      // base price rounded to 2 decimals, then returned
      const baseprice = parseFloat(pricing[style][sizing][qty].replace('$', '')) + extraCost
      const rounded = Math.round(baseprice * 100) / 100
      return rounded
    },
    staticExtras () {
      let cost = 0.0
      if (this.pinType.key === 'offsetPrinted') {
        cost += 100.0
      }
      if (this.pinType.key === 'silkScreen') {
        cost += 20.0
      }
      for (let i = 0; i < this.pinAddons.length; i++) {
        if (this.pinAddons[i].name === '3D Mold' || this.pinAddons[i].name === 'Zinc Alloy Mold' || this.pinAddons[i].name === 'Custom Backing') {
          cost += parseFloat(this.pinAddons[i].cost.replace('$', ''))
        }
      }
      return cost
    }
  },
  methods: {
    concatKeys ({ name, cost }) {
      return `${name} — [${cost}]`
    },
    handleChange (way, index) {
      if (way === 1) {
        this.addonOptions[index].qty += 1
      }
      if (way === 0 && this.addonOptions[index].qty !== 1) {
        this.addonOptions[index].qty -= 1
      }
    }
  }
}
</script>
