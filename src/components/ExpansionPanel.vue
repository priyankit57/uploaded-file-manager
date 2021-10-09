<template>
    <div class="expansion-panel">
        <v-expansion-panels multiple>
            <!-- <v-expansion-panel>
                <v-expansion-panel-header>
                    <div class="cancel-upload">
                        <div>
                            Temp
                        </div>
                        
                        
                    </div>
                </v-expansion-panel-header>
                <v-expansion-panel-content>
                    <template v-for="(file, i) in temporaryFiles">
                        <Alert :file="file" :key="i" :index="i" :loadingProgress="loadingProgress" :dismissible="true" :loadingState="'incomplete'" v-on:closeAlert="closeAlert"/>
                    </template>
                </v-expansion-panel-content>
            </v-expansion-panel> -->
            <v-expansion-panel>
                <v-expansion-panel-header>
                    <div class="cancel-upload">
                        <div>
                            Uploading
                        </div>
                        
                        <template v-if="uploadingFiles.length > 0">
                            <div style="font-size:8px;">
                                <a href="#"  v-on:click="cancelUpload">CANCEL UPLOAD</a>
                            </div>
                        </template>
                    </div>
                </v-expansion-panel-header>
                <v-expansion-panel-content>
                   
                    <v-alert v-for="(file,i) in uploadingFiles" :key="i"
                        color="#F6F7FC"
                        height="79"
                        :icon="file.type"
                        
                    >
                        <div class="alert-box">
                            <div>
                                <div class="file-name">{{file.name}}</div>
                                <div class="file-size">{{file.size}}</div>
                            </div>
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
                        </div>

                    </v-alert>
                    
                </v-expansion-panel-content>
            </v-expansion-panel>
            <v-expansion-panel>
                <v-expansion-panel-header>
                    <div class="cancel-upload">
                        <div>
                            Next Up
                        </div>
                        
                        <template v-if="nextUpFiles.length > 0">
                            <div style="font-size:8px;">
                                <a href="#"  v-on:click="cancelAll">CANCEL All</a>
                            </div>
                        </template>
                    </div>
                </v-expansion-panel-header>
                <v-expansion-panel-content>
                      
                    <v-alert v-for="(file,i) in nextUpFiles" :key="i"
                        dismissible
                        color="#F6F7FC"
                        height="79"
                        :icon="file.type"
                        close-icon="mdi-close"
                        v-model="visibility"
                        @input="closeAlert('next_up', i)"                       
                    >
                        <div class="alert-box">
                            <div>
                                <div class="file-name">{{file.name}}</div>
                                <div class="file-size">{{file.size}}</div>
                            </div>
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
                        </div>

                    </v-alert>
                    
                </v-expansion-panel-content>
            </v-expansion-panel>
            <v-expansion-panel>
                <v-expansion-panel-header>
                    <div class="cancel-upload">
                        <div>
                            Completed
                        </div>
                        
                        <template v-if="completedFiles.length > 0">
                            <div style="font-size:8px;">
                                <a href="#"  v-on:click="cancelComplete">DISMISS All</a>
                            </div>
                        </template>
                    </div>
                </v-expansion-panel-header>
                <v-expansion-panel-content>
                    <template v-if="completedFiles.length < 1">
                        <div class="sm-text-8">
                            No file uploads completed yet.
                        </div>
                    </template>  
                      
                    <v-alert v-for="(file,i) in completedFiles" :key="i"
                        dismissible
                        color="#F6F7FC"
                        height="79"
                        :icon="file.type"
                        close-icon="mdi-close"
                        v-model="visibility"
                        @input="closeAlert('completed', i)"
                    >
                        <div class="alert-box">
                            <div>
                                <div class="file-name">{{file.name}}</div>
                                <div class="file-size">{{file.size}}</div>
                            </div>
                            
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
                        </div>

                    </v-alert>
                    
                </v-expansion-panel-content>
            </v-expansion-panel>
            <v-expansion-panel>
                <v-expansion-panel-header>
                    Incomplete Uploads
                </v-expansion-panel-header>
                <v-expansion-panel-content>
                    <template v-if="inCompletedFiles.length < 1">
                        <div class="sm-text-8">
                            No incomplete uploads yet.
                        </div>
                    </template>
                    <template v-if="inCompletedFiles.length > 0">
                        <div class="incomplete-reset">
                            <div style="font-size:8px;" class="retry-incomplete">
                                <a href="#"  v-on:click="retryIncomplete">RETRY All</a>
                            </div>
                            <div style="font-size:8px;" class="dismiss-incomplete">
                                <a href="#"  v-on:click="dismissIncomplete">DISMISS All</a>
                            </div>
                        </div>
                    </template>  
                    <v-alert v-for="(file,i) in inCompletedFiles" :key="i"
                        dismissible
                        color="#F6F7FC"
                        height="79"
                        :icon="file.type"
                        close-icon="mdi-close"
                        v-model="visibility"
                        @input="closeAlert('incomplete', i)"
                    >
                        <div class="alert-box">
                            <div>
                                <div class="file-name">{{file.name}}</div>
                                <div class="file-size">{{file.size}}</div>
                            </div>
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
                            
                            
                        </div>

                    </v-alert>
                    
                </v-expansion-panel-content>
            </v-expansion-panel>
        </v-expansion-panels>
    </div>
</template>
<style scoped>
.expansion-panel {
    margin: auto;
    margin-left: 35px;
    margin-top:32px;
    width: 305px;
}

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

.cancel-upload {
    display:flex;
    justify-content: space-between;
    align-items: baseline;
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

.incomplete-reset {
    display: flex;
    justify-content: space-between;
}

.dismiss-incomplete {
    margin-right: 5px;
}

.retry-incomplete {
    margin-left: 5px;
}


</style>
<script>
import Alert from './Alert';

export default {
    name: "ExpansionPanel",

    components: {
        Alert,
    },

    props: {
        uploadingFiles: [],
        nextUpFiles: [],
        inCompletedFiles: [],
        completedFiles: [],
        temporaryFiles: [],
        loadingProgress: Number,
    },

    data () {
      return {
        visibility: true,
      }
    },
    
    methods: {
        cancelUpload (event) {
            console.log(this.visibility);
            console.log(event);
            this.$emit('cancelUploading', "upload canceled");
        },

        cancelAll (event) {
            console.log(event);
            this.$emit('cancelAll', "all canceled");
        },

        cancelComplete () {
            this.$emit('cancelCompleted', "dismiss upload completed files");
        },

        dismissIncomplete () {
            this.$emit('dismissIncomplete', "dismiss incomplete files");
        },

        retryIncomplete () {
            this.$emit('retryIncomplete', "retry incomplete files");
        },

        closeAlert(val) {
          
            this.$emit('dismissAlert', val);
        }
    },
    
}
</script>