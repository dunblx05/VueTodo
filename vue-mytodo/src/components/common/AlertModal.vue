<template>
<div class="modal modal__dim" v-on:click.self="$emit('close')">
    <div class="modal__content">
        <p class="modal__text">
            <slot name="modal-text">내용</slot>
        </p>
    <button class="modal__close" v-on:click="$emit('close')">OK</button>
    </div>
</div>
</template>

<script></script>