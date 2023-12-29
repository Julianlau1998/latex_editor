<template>
    <div class="is-topnav p-3 pb-4 pt-4">
        <div class="columns is-mobile">
            <div class="column ml-4 is-pointer">
                <span @click="home">
                    tex-Editor
                </span>
            </div>
            <div class="column settingsWrapper">
                <div v-if="settings" class="settingsItems settings">
                    <label v-if="helpAvailable" class="label is-pointer setting noselect">
                        <input @change="fileInput" accept=".tex, .txt, .aux, .toc, .lof, .lot, .log, .pdf" type="file" required/>
                        <span>
                            Import File
                        </span>
                    </label>
                    <span v-if="helpAvailable">
                        <div class="hr" />
                        <span @click="save" class="is-pointer mt-6 setting noselect">
                            Save as File
                        </span>
                    </span>
                    <!-- <span v-if="shareAvailable && helpAvailable">
                        <div class="hr" />
                        <span @click="share" class="is-pointer mt-6 setting noselect">
                            Share File
                        </span>
                    </span> -->
                    <!-- <span v-if="helpAvailable">
                        <div class="hr" />
                        <span @click="help" class="is-pointer mt-6 setting noselect">
                            Help
                        </span>
                    </span> -->
                    <span v-if="iosLiteApp">
                          <div class="hr" />
                          <span
                              @click="webviewTrigger"
                              class="is-pointer mt-6 setting noselect is-playBillingSetting"
                          >
                              Get Rid Of Ads
                          </span>
                    </span>
                    <span v-if="playBillingSupported">
                        <div class="hr" />
                        <span
                            @click="makePurchase()"
                            class="is-pointer mt-6 setting noselect is-playBillingSetting"
                        >
                            Support the Developer
                        </span>
                    </span>
                </div>
                <i
                    v-if="helpAvailable || playBillingSupported"
                    class="fas fa-ellipsis-v settings-icon is-pointer"
                    @click="settings=!settings"
                />
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TopNav',
    props: {
        helpAvailable: {
            required: false,
            default: true
        }
    },
    data () {
        return {
            settings: false,
            shareAvailable: false,
            playBillingSupported: false
        }
    },
    created () {
        this.checkPlayBillingAvailable()
        if(navigator.share !== undefined) {
            this.shareAvailable = true
        }
    },
    computed: {
      iosLiteApp () {
        return window.webkit && window.webkit.messageHandlers
      }
    },
    methods: {
        share () {
            this.$emit('share')
            this.settings = false
        },
        save () {
            this.$emit('save')
            this.settings = false
        },
        fileInput (event) {
            const file = event.target.files[0];
            const reader = new FileReader();
            reader.onload = e => this.$emit('fileInput', e.target.result);
            reader.readAsText(file);
            this.settings = false
        },
        async checkPlayBillingAvailable () {
            if ('getDigitalGoodsService' in window) {
                const service = await window.getDigitalGoodsService('https://play.google.com/billing');
                if (service) {
                    this.playBillingSupported = true
                }
            }
        },
        help () {
            this.$router.push('/help')
        },
        home () {
            if (this.$route.path === '/') return
            this.$router.push('/')
        },
        webviewTrigger () {
          if (this.iosLiteApp && window.webkit.messageHandlers.webviewTrigger) {
            window.webkit.messageHandlers.openAppStore.postMessage({
              "message": 'openAppStore'
            })
          }
        },
        async makePurchase(service) {
            const paymentMethods = [{
                supportedMethods: "https://play.google.com/billing",
                data: {
                    sku: 'support',
                }
            }]
            const paymentDetails = {
                total: {
                    label: `Total`,
                    amount: {currency: `USD`, value: `5`}
                }
            }
            const request = new PaymentRequest(paymentMethods, paymentDetails);
            try {
                const paymentResponse = await request.show();
                const {purchaseToken} = paymentResponse.details;
                await service.acknowledge(purchaseToken, 'repeatable');
            } catch(e) {
                alert('Something went wrong. Please try again.')
            }
        }
    }
}
</script>