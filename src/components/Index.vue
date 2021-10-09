<template>
    <v-app>
        <v-main class="main">
            <Header v-on:headerToIndex="onUploadBtnClick"/>
            <ExpansionPanel 
                v-on:cancelUploading="onCancelUploadClick"
                v-on:dismissAlert="onDismissAlertClick" 
                v-on:cancelAll="onCancelAllClick" 
                v-on:cancelCompleted="onCancelCompletedClick" 
                v-on:dismissIncomplete="onDismissIncompleteClick" 
                v-on:retryIncomplete="onRetryIncompleteClick" 
                :nextUpFiles="nextUpFiles" 
                :uploadingFiles="uploadingFiles" 
                :completedFiles="completedFiles" 
                :inCompletedFiles="inCompletedFiles" 
                :loadingProgress="loadingProgress"
                :temporaryFiles="temporaryFiles"
            />
        </v-main>
    </v-app>
</template>

<style scoped>
.main {
    width: 375px;
}

</style>

<script>
import Header from './Header';
import ExpansionPanel from './ExpansionPanel';

export default {
    name: 'App',

    components: {
        Header,
        ExpansionPanel,
    },

    

    data: () => ({
        uploadingFiles: [],
        nextUpFiles: [],
        inCompletedFiles: [],
        completedFiles: [],
        loadingProgress: 0,
        temporaryFiles: [
            { type: "fa fa-image green--text", name: "Brithday 2020.png", size: "2.0 MB"},
            { type: "fa fa-file-zip-o yellow--text", name: "Students 2020.zip", size: "2.0 MB"},
            { type: "fa fa-file-pdf-o red--text", name: "License.pdf", size: "2.0 MB"},
            { type: "fa fa-file-excel-o green--text", name: "PayRoll.xlas", size: "2.0 MB"}
        ],
        interval: 0,
    }),

    methods: {
        // Triggered when `headerToIndex` event is emitted by the child.
        onUploadBtnClick () {
            var flag = Math.floor(Math.random() * 4);
           
            if (this.uploadingFiles.length > 0)
            {
                this.nextUpFiles.push(this.temporaryFiles[flag]);
            } else {
                this.uploadingFiles.push(this.temporaryFiles[flag]);
                this.initPrecess();
            }
            
            
        },

        initPrecess() {
            this.interval = setInterval(() => {

                if (this.loadingProgress == 75) {
                    var flag = Math.floor(Math.random() * 2);
                    
                    
                    if (flag == 0) {
                        clearTimeout(this.interval)
                        this.manageUploadedFile(false);
                    }
                // return;
                }

                if (this.loadingProgress >= 100) {
                    clearTimeout(this.interval);
                    this.manageUploadedFile(true);
                }
                this.loadingProgress += 3 
            }, 300)
        },

        manageUploadedFile(completed) {
            this.loadingProgress = 0;

            if (completed)
            {
                this.completedFiles.push(this.uploadingFiles[0]);
            } else {
                this.inCompletedFiles.push(this.uploadingFiles[0]);
            }

            this.uploadingFiles.pop();
            
            if (this.nextUpFiles.length == 0)
            {
                console.log("uploading is ended.");
                clearTimeout(this.interval);
                return;
            }
            
            this.uploadingFiles.push(this.nextUpFiles.shift());
            this.initPrecess();
        },

        onCancelUploadClick () {
            this.loadingProgress = 0;
            
            this.inCompletedFiles.push(this.uploadingFiles.pop());
                        
            if (this.nextUpFiles.length == 0)
            {
                console.log("uploading is ended.")
                clearTimeout(this.interval);
                return;
            }
            
            this.uploadingFiles.push(this.nextUpFiles.shift());
          
            clearTimeout(this.interval);
            this.initPrecess();
        
        },

        onCancelAllClick () {
            this.nextUpFiles = [];
        },

        onCancelCompletedClick () {
            this.completedFiles = [];
        },

        onDismissIncompleteClick () {
            this.inCompletedFiles = [];
        },

        onRetryIncompleteClick () {
           
            this.nextUpFiles = this.nextUpFiles.concat(this.inCompletedFiles)
            this.inCompletedFiles = [];
            
            if (this.uploadingFiles.length == 0)
            {
                this.uploadingFiles.push(this.nextUpFiles.shift());
                this.initPrecess();
            }
           
        },
        onDismissAlertClick (val) {
            console.log(val.index);
            alert("click");
            // if (val.status == "next_up")
            // {
            //     console.log(this.nextUpFiles.length);
            //     this.nextUpFiles.splice(val.index, 1);
            //     console.log(this.nextUpFiles.length);
                               
            // }

            // if (val.status == "completed")
            // {
            //     console.log(this.completedFiles.length);
            //     this.completedFiles.splice(val.index, 1);
            //     console.log(this.completedFiles.length);
               
            // }

            // if (val.status == "incomplete")
            // {
            //     console.log(this.inCompletedFiles.length);
            //     this.inCompletedFiles.splice(val.index, 1);
            //     console.log(this.inCompletedFiles.length);
               
            // }
        },


    }

    
};
</script>
