<template>
    <button type="button" class="button">
        <slot></slot>
    </button>
</template>
<script>
export default {
    name: 'calc-button'
}
</script>
<style scoped>
.button {
    color: #eee;
    background: #314967;
    font: 30px/50px Arial;
    text-align: center;
    box-shadow: 1px 1px 5px rgba(0,0,0,0.5);
    border-radius: 5px;
    user-select: none;
    position: relative;
    z-index: 1;
}
.button:hover {
    cursor: pointer;
}
.button:active {
    box-shadow: inset 1px 1px 5px rgba(0,0,0,0.5);
}
.actions {
    font-size: 40px;
}
.clear {
    background: #ff2400;
    font-weight: bold;
}
.eval {
    background: #00844a;
    font-size: 50px;
    grid-column: 5/7;
}
.backspace {
    background: #ffd700;
    color: #314967
}
.digit {
    background: #eee;
    color: #314967;
}
</style>
