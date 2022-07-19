<template>
  <div class="static">
    <div id="map" class="relative w-full"></div>

    <div class="absolute text-white p-5 pl-96 bottom-0 left-0 w-full text-3xl">
      <div class="absolute text-white p-4 bottom-0 left-0 w-56">
        <img :src="require(`@/static/icons/homeQgo.svg`)" />
      </div>
      <div class="absolute text-white p-4 bottom-0 right-0 w-56">
        <img :src="require(`@/static/icons/the-sustainables.svg`)" />
      </div>
    </div>
    <div class="absolute text-white p-4 pl-24 pt-12 top-0 left-0 w-2/5">
      <h2 class="ml-4 text-3xl font-semibold">
        Let’s make the Netherlands green again
      </h2>
      <div class="w-full flex items-stretch p-2">
        <div class="w-6/12 p-2">
          <div class="text-white border-2 p-4 border-green-500 rounded-xl">
            <img
              :src="require(`@/static/icons/green.svg`)"
              class="mb-3 w-full"
            />
            <div class="grid grid-cols-3 gap-1">
              <div>
                <span class="text-5xl w-32 text-green-600"
                  >{{
                    numberWithCommas(
                      Math.round(
                        (estimatedTotalElectricitySavings * 0.649) / 1000,
                        2
                      )
                    )
                  }}
                </span>
              </div>
              <div class="col-span-2">
                <span class="text-sm">ton co2 emission prevented</span>
              </div>
            </div>
          </div>
          <div
            class="text-white border-2 mt-3 p-4 border-orange-400 rounded-xl"
          >
            <div border-red-500 rounded-xl>
              <div class="grid grid-cols-3 gap-1">
                <div>
                  <span class="text-4xl w-32 text-orange-500"
                    >{{ totalInstalations }}
                  </span>
                </div>
                <div class="col-span-2">
                  <span class="text-sm">installations were done in total</span>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="text-white w-6/12 p-2">
          <div class="flex flex-col border-2 border-blue-400 h-full rounded-xl">
            <div class="m-auto p-3">
              <img
                :src="require(`@/static/icons/blue.svg`)"
                class="mb-2 mt-6 w-full h-32"
              />
            </div>
            <div class="m-auto">
              <span class="text-5xl w-32 text-blue-500"
                >{{
                  numberWithCommas(
                    Math.round(estimatedTotalElectricitySavings, 2)
                  )
                }}
                kWh
              </span>
            </div>
            <div class="m-auto mb-6">
              <p class="text-center">electricity usage reduced</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import jsonData from '~/static/localizaciones.json'
export default {
  data() {
    return {
      estimatedTotalElectricitySavings: 0,
      totalInstalations: 0,
      markers: [],
      map: null,
    }
  },
  mounted() {
    // asdf

    // const { data: solarPanelInstallations } = await this.$axios.get('/customers')
    const solarPanelInstallations = jsonData
    const mapboxgl = require('mapbox-gl')
    this.totalInstalations = solarPanelInstallations.length

    // const data = JSON.stringify({
    //     "dataSource": "Cluster0",
    //     "database": "localizaciones",
    //     "collection": "localizacion"
    //     });

    // const totalLocalizaciones = await this.$axios.post(data);
    // console.log(totalLocalizaciones);

    // const bounds = [
    //   [0.262701, 50.746976], // Southwest coordinates
    //   [8.643672, 53.857353], // Northeast coordinates
    // ]

    this.map = new mapboxgl.Map({
      accessToken:
        'pk.eyJ1Ijoic3ViZXhwdWVzdGEiLCJhIjoiY2w1cnRjcHlhMDAxeDNibWswbWNxc3ZrZiJ9.pCBAx6YUjgSnf0ItJcQwwQ',
      container: 'map',
      style: 'mapbox://styles/subexpuesta/cl5rug7x1000314qtycqf6cqm',
      center: [-3.70256, 40.4165],
      zoom: 3,
      pitch: 0,
    })

    // add markers to map
    // for (const solarPanel of solarPanelInstallations) {
    //     if(this.goodData(solarPanel)){
    //     this.estimatedTotalElectricitySavings += solarPanel.products[0].estimatedTotalElectricitySavings

    //     let popup = `<div class="rounded overflow-hidden shadow-lg"> <img class="w-full" src="${require('@/static/icons/house.png')}" alt="Sunset in the mountains"> <div class="px-6 py-4"> `
    //     popup += `<div class="font-bold text-xl mb-2">We installed ${solarPanel.products[0].quantity} solar panels!</div> `
    //     popup += `<p class="text-gray-700 text-base"> This installation generates <span class="font-semibold text-green-700">${Math.round(Math.abs(solarPanel.products[0].totalElectricity),2)}</span> kWh of solar energy per year, and has already generated <span class="font-semibold text-green-700">${this.numberWithCommas(Math.round(solarPanel.products[0].estimatedTotalElectricitySavings, 2))}</span> kWh since installation. </p> <p> Via this installation, <span class="font-semibold text-blue-700">${this.numberWithCommas(Math.round(solarPanel.products[0].estimatedTotalElectricitySavings*0.649/1000, 2))}</span> ton of CO2 has been prevented.</p> </div> `
    //     popup += `<div class="px-6 pt-4 pb-2"> <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 inline" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"> <path stroke-linecap="round" stroke-linejoin="round" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z" /> <path stroke-linecap="round" stroke-linejoin="round" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" /> <span class="text-sm font-semibold text-gray-700"> ${solarPanel.address.city}</span> </svg>`
    //     const el = document.createElement('div');
    //     el.className = 'marker';
    //     el.style.backgroundImage = `url(${require('@/static/icons/marker_big.svg')})`
    //     el.style.width = '20px'
    //     el.style.height = '20px'

    //     el.addEventListener('click', (e) => {
    //     if (this.selectedMarker) {
    //       this.selectedMarker.classList.remove('hidden')
    //     }
    //     this.selectedMarker = e.target
    //     this.selectedMarker.classList.add('hidden')
    //   })

    //   const popupElement = new mapboxgl.Popup({
    //     offset: 0,
    //     anchor: 'bottom',
    //   }).setHTML(popup)

    //     popupElement.on('close', () => {
    //     if (this.selectedMarker) {
    //       this.selectedMarker.classList.remove('hidden')
    //     }
    //   })
    //     new mapboxgl.Marker(el).setLngLat([solarPanel.address.longituide, solarPanel.address.latitude]).setPopup(popupElement).addTo(this.map);

    //     }
    // }
  },
  methods: {
    numberWithCommas(x) {
      return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
    },
    goodData(solarPanel) {
      if (
        !(
          Object.keys(solarPanel).length === 0 &&
          solarPanel.constructor === Object
        ) &&
        solarPanel.products !== 'undefined' &&
        solarPanel.products.length > 0 &&
        solarPanel.address !== 'undefined'
      ) {
        return true
      } else {
        return false
      }
    },
  },
}
</script>
<style scoped>
#map {
  min-height: 100vh;
}

::v-deep .mapboxgl-popup-content {
  width: 387px !important;
  padding: 0 !important;
  border-radius: 5px;
}

::v-deep .mapboxgl-popup-close-button {
  visibility: hidden;
}

::v-deep .mapboxgl-ctrl-bottom-right {
  visibility: hidden;
}

::v-deep .mapboxgl-ctrl-bottom-left {
  visibility: hidden;
}
</style>
