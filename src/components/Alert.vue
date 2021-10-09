<template>
<div>
    <v-alert 
        color="#F6F7FC"
        height="79"
        :icon="file.type"
        close-icon="mdi-close"
    >
        <div class="alert-box">
            <div>
                <div class="file-name">{{file.name}}</div>
                <div class="file-size">{{file.size}}</div>
            </div>
            <template v-if="loadingState === 'progress'">
                <div class="loading-state">
                    <div class="loading-mark">
                        <v-progress-circular
                        :rotate="360"
                        :size="24"
                        :width="3"
                        :value="loadingProgress"
                        color="teal"
                        >
                        </v-progress-circular>
                    </div>
                    <div style="font-size:8px;">
                        Encrypting..
                    </div>
                </div>
            </template>
            <template v-if="loadingState === 'nextUp'">
                <div class="loading-state">
                    <div class="loading-mark">
                        <v-progress-circular
                        :rotate="360"
                        :size="24"
                        :width="3"
                        :value="0"
                        color="teal"
                        >
                        </v-progress-circular>
                    </div>
                    <div style="font-size:8px;">
                        Waiting..
                    </div>
                </div>
            </template>
            <template v-if="loadingState === 'completed'">
                <div class="loading-state">
                    <div class="loading-mark">
                        <v-icon
                            color="green"
                        >
                            mdi-checkbox-marked-circle
                        </v-icon>
                    </div>
                    <div style="font-size:8px;">
                        Done
                    </div>
                </div>
            </template>
            <template v-if="loadingState === 'incomplete'">
                 <div class="loading-state">
                    <div class="loading-mark">
                        <v-icon
                            color="red"
                            style="font-size:20px;"
                            >
                            fas fa-redo-alt
                        </v-icon>
                    </div>
                    <div style="font-size:8px;">
                        Canceled
                    </div>
                </div>
            </template>
        </div>
        <template v-slot:close="{ toggle }" v-if="dismissible">
            <v-icon
                @click="myFunction(toggle)" 
            >
            mdi-close
            </v-icon>
        </template>

    </v-alert>
    <!-- <div class="text-center">
      <v-btn
        v-if="!visibility"
        color="deep-purple accent-4"
        dark
        @click="visibility = true"
      >
        Reset
      </v-btn>
    </div> -->
</div>
</template>
<style scoped>

.alert-box {
    display: flex;
    justify-content: space-between;
}


.file-name {
    margin-top: 5px;
    font-weight: 500;
    font-size: 12px;
    line-height: 5px;
}

.file-size {
    margin-top: 15px;
    font-weight: normal;
    font-size: 8px;
    line-height: 5px;
}

.sm-text-8 {
    width: 122px;
    /* font-family: Poppins; */
    font-style: normal;
    font-weight: normal;
    font-size: 8px;
    line-height: 10px;
    color: #9E9E9E;
}

.loading-state {
    display:flex; 
    flex-direction:column;
    justify-content: center;
}

.loading-mark {
    display: flex;
    justify-content: center;
}



</style>
<script>
    export default {
        props: {
            file: {},
            index: Number,
            tabName: String,
            loadingState: String,
            dismissible: Boolean,
            loadingProgress: Number,
        },

        data () {
            return {
                visibility: true,
            }
        },
        // watch: {
        //     visibility (val) {
        //         console.log(val);
        //         if (val) {
        //             // Custom action when the alert was shown
        //         } else {
        //             // Custom action when the alert was shown
        //         }
        //         this.$emit('closeAlert', {status: 'temp', index: 1});
        //     }
        // },
        methods: {
            myFunction (toggle) {
            //    toggle();
            console.log(toggle);

            this.$emit('closeAlert', {status: 'temp', index: this.index});
            },
        }
    }
</script>