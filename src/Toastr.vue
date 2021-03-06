<template>
    <transition :enter-active-class="enterActiveClass" :leave-active-class="leaveActiveClass" @before-enter="beforeEnter" @after-enter="afterEnter" @before-leave="beforeLeave">
        <div class="toast" :class="['toast-'+type]" :style="{backgroundColor:toastBackgroundColor}" v-if="show">
            <button class="toast-close-button" role="button" @click="hideToastr" v-if="closeButton">×</button>
            <div class="toast-title">{{title}}</div>
            <div class="toast-message">{{message}}</div>
        </div>
    </transition>
</template>


<script>
export default {
    name: 'CxltToastr',
    data: () => {
        return {
            show: false
        }
    },
    props: {
        type: {
            type: String,
            default: 'success'
        },
        position: {
            type: String,
            default: 'top center'
        },
        title: {
            type: String
        },
        message: {
            type: String
        },
        closeButton: {
            type: Boolean,
            default: true
        },
        timeOut: {
            default: '1500'
        },
        showMethod: {
            type: String,
            default: 'fadeIn'
        },
        hideMethod: {
            type: String,
            default: 'fadeOut'
        },
        showDuration: {
            default: '1000'
        },
        hideDuration: {
            default: '1000'
        },
        delay: {
            default: '0'
        },
        successColor: {
            type: String
        },
        infoColor: {
            type: String
        },
        warningColor: {
            type: String
        },
        errorColor: {
            type: String
        },
        color: {
            type: String
        }
    },
    beforeMount() {
        let toastContainer = document.querySelector(`.cxlt-toastr-container.toast-${this.positionClass}`)
        if (!toastContainer) {
            toastContainer = document.createElement('div')
            // 分2次添加，是为了兼容IE10
            toastContainer.classList.add('cxlt-toastr-container')
            toastContainer.classList.add(`toast-${this.positionClass}`)
            document.body.appendChild(toastContainer)
        }
        toastContainer.appendChild(this.$el)
    },
    mounted() {
        setTimeout(() => this.showToastr(), this.delay)
    },
    computed: {
        positionClass() {
            return this.position.split(' ').join('-')
        },
        enterActiveClass() {
            return 'animated ' + this.showMethod
        },
        leaveActiveClass() {
            return 'animated ' + this.hideMethod
        },
        toastBackgroundColor() {
            if (this.color) {
                return this.color
            } else {
                if (this.type === 'success' && this.successColor) {
                    return this.successColor
                } else if (this.type === 'info' && this.infoColor) {
                    return this.infoColor
                } else if (this.type === 'warning' && this.warningColor) {
                    return this.warningColor
                } else if (this.type === 'error' && this.errorColor) {
                    return this.errorColor
                }
                return null
            }
        }
    },
    methods: {
        showToastr() {
            this.show = true
            this.sto = setTimeout(() => this.hideToastr(), this.timeOut)
        },
        hideToastr() {
            clearTimeout(this.sto)
            this.show = false
        },
        beforeEnter(el) {
            el.style.animationDuration = this.showDuration + 'ms'
        },
        afterEnter(el) {
            this.$el.classList.add('animated')
            this.$el.classList.add('fadeIn')
        },
        beforeLeave(el) {
            el.style.animationDuration = this.hideDuration + 'ms'
        }
    }
}

</script>
