<template>
    <div class="expansion-panel">
        <v-expansion-panels multiple>
           
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
                    <template v-for="(file,i) in uploadingFiles">
                        <Alert :file="file" :key="i" :dismissible="false" :loadingState="'uploading'" :index="i" :loadingProgress="loadingProgress" v-on:closeAlert="closeAlert"/>
                    </template>                      
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
                      
                    <template v-for="(file,i) in nextUpFiles">
                        <Alert :file="file" :key="i" :dismissible="true" :loadingState="'nextUp'" :index="i" :loadingProgress="loadingProgress" v-on:closeAlert="closeAlert"/>
                    </template>
                    
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
                      
                    <template v-for="(file,i) in completedFiles">
                        <Alert :file="file" :key="i" :dismissible="true" :loadingState="'completed'" :index="i" :loadingProgress="loadingProgress" v-on:closeAlert="closeAlert"/>
                    </template>
                    
                </v-expansion-panel-content>
            </v-expansion-panel>
            <v-expansion-panel>
                <v-expansion-panel-header>
                    Incomplete Uploads
                </v-expansion-panel-header>
                <v-expansion-panel-content>
                    <template v-if="incompleteFiles.length < 1">
                        <div class="sm-text-8">
                            No incomplete uploads yet.
                        </div>
                    </template>
                    <template v-if="incompleteFiles.length > 0">
                        <div class="incomplete-reset">
                            <div style="font-size:8px;" class="retry-incomplete">
                                <a href="#"  v-on:click="retryIncomplete">RETRY All</a>
                            </div>
                            <div style="font-size:8px;" class="dismiss-incomplete">
                                <a href="#"  v-on:click="dismissIncomplete">DISMISS All</a>
                            </div>
                        </div>
                    </template>  
                    <template v-for="(file,i) in incompleteFiles">
                        <Alert :file="file" :key="i" :dismissible="true" :loadingState="'incomplete'" :index="i" :loadingProgress="loadingProgress" v-on:closeAlert="closeAlert"/>
                    </template>
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

.cancel-upload {
    display: flex;
    justify-content: space-between;
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

.sm-text-8 {
    width: 122px;
    /* font-family: Poppins; */
    font-style: normal;
    font-weight: normal;
    font-size: 8px;
    line-height: 10px;
    color: #9E9E9E;
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
        incompleteFiles: [],
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