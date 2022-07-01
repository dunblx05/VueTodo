<template>
    <div class="title">
        <p class="title__text">
            <span class="title__message">Good {{ message }}, </span>
            <span v-on:keyup.enter="handleName"
                v-on:blur="handleBlur"
                class="title__name"
                ref="test"
                contenteditable="true">{{ propName }}
            </span>
        </p>
        <p class="title__task">
            <span class="title__task-top">You've  </span>
            <span class="title__task-count">
                <em class="title__task-left">{{ propCount.left }}</em>
                <em v-if="propCount.total" class="title__task-total">/{{ propCount.total }}</em>
            </span>
            <span class="title__task-bottom"> tasks today !</span>
            <span class="title__task-info"></span>
        </p>
    </div>
</template>

<script>
import getDate from "../assets/commonJS/getDate.js"

export default {
    props : ["propCount", "propName"],
    data() {
        return {
            message : ""
        }
    },
    methods: {
        handleBlur(e) {
            const originalName = this.propName;
            const newName = e.target.innerText;
            if(newName !== originalName) {
                if(newName === "") {
                    e.target.innerText = originalName
                }
                else{
                    this.$emit("changeName", newName)
                }
            }
        },
        handleName() {
            this.$refs.test.blur();
        }
    },
    mounted() {
        this.message = getDate().daytime;
    }
}
</script>
