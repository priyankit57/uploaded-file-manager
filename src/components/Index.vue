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
                :incompleteFiles="incompleteFiles" 
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
        incompleteFiles: [],
        completedFiles: [],
        loadingProgress: 0,
        temporaryFiles: [
            { type: "fa fa-image green--text", name: "Brithday 2020.png", size: "2.0 MB"},
            { type: "fa fa-file-zip-o yellow--text", name: "Students 2020.zip", size: "2.0 MB"},
            { type: "fa fa-file-pdf-o red--text", name: "License.pdf", size: "2.0 MB"},
            { type: "fa fa-file-excel-o green--text", name: "PayRoll.xlas", size: "2.0 MB"},
            { type: "fa fa-image green--text", name: "Portfolio.png", size: "2.0 MB"},
            { type: "fa fa-file-zip-o yellow--text", name: "IP addresses 2020.zip", size: "2.0 MB"},
            { type: "fa fa-file-pdf-o red--text", name: "Phone Number.pdf", size: "2.0 MB"},
            { type: "fa fa-file-excel-o green--text", name: "Excel.xlas", size: "2.0 MB"}
        ],
        interval: 0,
    }),

    methods: {
        // Triggered when `headerToIndex` event is emitted by the child.
        onUploadBtnClick () {
            var flag = Math.floor(Math.random() * 8);
           
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
                this.completedFiles.unshift(this.uploadingFiles[0]);
            } else {
                this.incompleteFiles.unshift(this.uploadingFiles[0]);
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
            
            this.incompleteFiles.unshift(this.uploadingFiles.pop());
                        
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
            console.log("cancel all");
            this.incompleteFiles = this.nextUpFiles.concat(this.incompleteFiles).slice();
            this.nextUpFiles = [];
        },

        onCancelCompletedClick () {
            this.completedFiles = [];
        },

        onDismissIncompleteClick () {
            this.incompleteFiles = [];
        },

        onRetryIncompleteClick () {
           
            this.nextUpFiles = this.nextUpFiles.concat(this.incompleteFiles)
            this.incompleteFiles = [];
            
            if (this.uploadingFiles.length == 0)
            {
                this.uploadingFiles.push(this.nextUpFiles.shift());
                this.initPrecess();
            }
           
        },
        onDismissAlertClick (val) {
           
            if (val.status == "nextUp")
            {
                this.incompleteFiles.unshift(this.nextUpFiles[val.index]);
                
                this.nextUpFiles.splice(val.index, 1);
            }

            if (val.status == "completed")
            {
                this.completedFiles.splice(val.index, 1);
            }

            if (val.status == "incomplete")
            {
                this.incompleteFiles.splice(val.index, 1);
            }
        },
    }
};
</script>
