<template>
    <div>
        <div class="is-modal-wrapper"></div>
        <div class="modal">
            <form>
                <h2>
                    Save File
                </h2>
                <input
                    v-model="fileName"
                    type="text"
                    placeholder="File Name"
                    class="input mt-2"
                    max="255"
                    min="1"
                    required
                    autofocus
                >
                <br>
                <button @click="close" class="button is-secondary pl-3 pr-3">
                    Close
                </button>
                <button type="submit" @click="onSubmit" class="button is-primary pl-4">
                    Save
                    <i class="fas fa-save pl-1 pr-2" /> 
                </button>
            </form>
        </div>
    </div>
</template>

<script>
export default {
    data () {
        return {
            fileName: ''
        }
    },
    methods: {
        onSubmit () {
            if (this.fileName.length) {
                this.$emit('save', this.fileName)
            }
        },
        close () {
            this.$emit('close')
        }
    }
}
</script>