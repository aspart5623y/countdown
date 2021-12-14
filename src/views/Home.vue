<template>
    <div>
        <h3>Hello World</h3>
        <Counter 
            :year="2021" 
            :month="11" 
            :date="13" 
            :hour="22" 
            :munite="29" 
            :second="58" 
            :millisecond="58" 
        />
    </div>
</template>

<script>
    import Counter from '@/components/Counter'
    export default {
        components: {
            Counter
        }
    }
</script>

<style lang="scss" scoped>

</style>
