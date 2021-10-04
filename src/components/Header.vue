<template>
    <div class="header-rect">
        <div class="status-bar">
            <div class="status-bar-left">
                <span class="current-time">
                    9:41
                </span>
            </div>
            <div class="status-bar-right">
                <i class="fa fa-signal signal"></i>
                <i class="fa fa-wifi wifi"></i>
                <i class="fa fa-battery-4 battery"></i>
            </div>
        </div>
        <div class="file-manager-bar">
            <div class="file-manger-center">
                Manage Files
            </div>
            <div>
                <v-file-input
                v-model="files"
                @change="uploadFiles"
                hide-input
                multiple
                truncate-length="6"
                prepend-icon="mdi-plus blue--text"
                loading="true"
                style="padding-top:0px;margin-top:0px;margin-right:10px;"                               
                >
                </v-file-input>
                
            </div>
        </div>
    </div>
</template>

<script>
export default {
    
}
</script>
<style scoped>


.header-rect {
    width: 375px;
    height: 95px;
    left: 0px;
    top: 0px;

    background: #FFFFFF;
    box-shadow: 0px 1px 10px rgba(0, 0, 0, 0.12);
}

.status-bar {
    padding-top: 9px;
    display: flex;
    justify-content: space-between;
}
.status-bar-left {
    
    width: 100px;
    height: 28px;
    display: flex;
    justify-content: center;
   
}

.status-bar-right {
    padding-right: 6px;
    width: 100px;
    height: 28px;
    display: flex;
    justify-content: center;

}


.current-time {
    margin-top: 7px;
    margin-left: 5px;
    width: 54px;
    height: 16px;
    
    /* font-family: Sofia Pro; */
    font-style: normal;
    font-weight: 600;
    font-size: 14px;
    line-height: 20px;
    display: flex;
    align-items: center;
    text-align: center;

    color: #000000;
}

.signal {
    margin-right: 5.02px;
    margin-top: 9px;
    width: 17px;
    height: 10.67px;
}

.wifi {
    margin-top: 9px;
    width: 15.27px;
    height: 10.97px;
   
}

.battery {
    margin-top: 9px;
    margin-left: 8.02px;
    width: 18px;
    height: 7.33px;
   
}

.file-manager-bar {
    margin-top: 22px;
    display: flex;
    justify-content: space-between;
    align-items: center;

}

.file-manger-center {
    /* margin: auto;    */
    margin-left: 141px;
    /* width: 93px;
    height: 21px; */
}


</style>

<script>
export default {
    
    data: () => ({
      files: [],
    }),
    methods: {
        remove (index) {
            this.files.splice(index, 1)
        },
        uploadFiles(event) { 
            console.log(this.files); 
            this.$emit('headerToIndex', this.files);
        }
    },

}
</script>