<template>
   <div>
    <v-alert
            dismissible
            color="#F6F7FC"
            height="79"
            icon="fa fa-image white--text green"
        >
            <div>Birthday 2020.PNG</div>
            <div>2.0 MB</div>

    </v-alert>
    <v-alert
            dismissible
            color="#F6F7FC"
            height="79"
            icon="fa fa-file-zip-o white--text yellow"
        >
            <div>Birthday 2020.PNG</div>
            <div>2.0 MB</div>

    </v-alert>
   </div>    
    
</template>
<style lang="scss" scoped>


</style>
<script>
    export default {
        data () {
            return {
                alert: true,
            }
        },
    }
</script>