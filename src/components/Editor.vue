<template>
  <div class="is-editor-wrapper">
        <div class="buttonsWrapper">
            <transition name="fade" mode="out-in">
                <span v-if="!preview" :key="1">
                    <button
                        @click="preview = !preview"
                        class="button is-primary mt-3 pl-4"
                    >
                        Preview
                        <i class="fas fa-eye pl-1 pr-2" /> 
                    </button>
                    <button
                        v-if="!preview && (!iOS || shareAvailable)"
                        :key="1"
                        class="button is-primary ml-4 pl-4"
                        @click="copyFormula"
                    >
                        Copy
                        <i class="fas fa-download pl-1 pr-2" /> 
                    </button>
                    <p v-if="copied">
                        copied!
                    </p>
                </span>
                <span v-else :key="2">
                    <button
                        @click="preview = !preview"
                        class="button is-primary mt-5 pl-4"
                    >
                        Edit
                        <i class="fas fa-highlighter pl-1 pr-2" /> 
                    </button>
                </span>
            </transition>
        </div>
        <br>
        <br>
        <transition name="fade" mode="out-in">
            <div v-if="!preview" :key="1">
                <textarea
                    v-model="inputText"
                    ref="textArea"
                    class="is-editor pt-5 pb-5 is-primary"
                    placeholder="Type your Formula here"
                />
                <br>
                <div class="is-button-wrapper">
                    <button @click="addToLatex('+')" class="is-tex-button">
                        +
                    </button>
                    <button @click="addToLatex('-')" class="is-tex-button">
                        -
                    </button>
                    <button @click="addToLatex('*')" class="is-tex-button">
                        *
                    </button>
                    <button @click="addToLatex('/')" class="is-tex-button">
                        /
                    </button>
                    <button @click="addToLatex('\\')" class="is-tex-button">
                        \
                    </button>
                    <button @click="addToLatex('\\sqrt(')" class="is-tex-button">
                        √
                    </button>
                    <button @click="addToLatex('\\over ')" class="is-tex-button">
                        x/y
                    </button>
                    <button @click="addToLatex('(')" class="is-tex-button">
                        (
                    </button>
                    <button @click="addToLatex(')')" class="is-tex-button">
                        )
                    </button>
                </div>
                <div class="mt-5">
                    <vue-mathjax
                        :save="false"
                        :formula="`$$ ${inputText} $$`"
                    />
                </div>
            </div>
            <div v-else :key="2" class="is-markdown-content">
                <div class="is-latex">
                    <vue-mathjax
                        :save="false"
                        :formula="`$$ ${inputText} $$`"
                    />
                </div>
            </div>
        </transition>
<!--        <button
            v-if="iosLiteApp"
            @click="webviewTrigger"
            class="button is-ads-button is-border-secondary mt-5"
        >
          Get Rid Of Ads
        </button>-->
        <SaveModal
            v-if="saveFileModalOpen"
            @save="downloadFile"
            @close="saveFileModalOpen=false"
        />
  </div>
</template>

<script>
import SaveModal from '@/components/modals/SaveModal'
import {VueMathjax} from 'vue-mathjax'

export default {
    name: 'Editor',
    components: {
        SaveModal,
        'vue-mathjax': VueMathjax
    },
    props: {
        share: {
            type: Boolean,
            required: true
        },
        save: {
            type: Boolean,
            required: true
        }
    },
    data () {
        return  {
            inputText: '',
            preview: false,
            markdownWrapper: '',
            saveFileModalOpen: false,
            shareAvailable: false,
            copied: false
        }
    },
    watch: {
        inputFile (val) {
            if (val != '') {
                this.inputText = val
                this.$store.state.inputFile = ''
            }
        },
        share (val) {
            if (val) this.shareFile()
        },
        save (val) {
            if (val) this.saveFileModal()
        }
    },
    computed: {
        iOS () {
            return this.$store.state.iOS
        },
        inputFile () {
            return this.$store.state.inputFile
        },
        iosLiteApp () {
          return window.webkit && window.webkit.messageHandlers
        }
    },
    created () {
        // window.visualViewport.addEventListener(
        //     'resize', 
        //     () => {
        //         if (this.iOS) {
        //             setTimeout(() => {
        //                 window.scrollTo(0)
        //             }, 200)
        //         }
        //     } 
        // )
        this.shareAvailable = window.navigator.share
    },
    methods: {
        addToLatex (element) {
            this.inputText += element
            this.$refs.textArea.focus()
        },
        saveFileModal () {
            if (!this.iOS) {
                this.saveFileModalOpen = true
                return
            }
            this.shareFile()
        },
        copyFormula () {
            navigator.clipboard.writeText(this.inputText)
                .then(() => {
                    this.copied = true
                    setTimeout(() => {
                        this.copied = false
                    }, 400)
                })
        },
        createFileLink (fileName) {
            const file = new Blob([this.inputText], { type: "data:text/csv;charset=utf-8" }, `${fileName}.tex`)
            return window.URL.createObjectURL(file)
        },
        downloadFile (fileName) {
            const link = this.createFileLink (fileName)

            let hiddenElement = document.createElement('a')
            hiddenElement.href = link
            hiddenElement.download = `${fileName}.tex`
            hiddenElement.click();  
            this.saveFileModalOpen = false
        },
        shareFile () {
            if (this.inputText.length) {
                navigator.share({
                    "title": 'Markdown File',
                    "text": this.inputText
                })
            }
        },
        webviewTrigger () {
          if (this.iosLiteApp && window.webkit.messageHandlers.webviewTrigger) {
            window.webkit.messageHandlers.webviewTrigger.postMessage({
              "message": 'open AppStore:'
            });
          }
        }
    }
}
</script>