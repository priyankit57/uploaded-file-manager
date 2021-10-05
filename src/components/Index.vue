<template>
    <v-app>
        <v-main class="main">
            <Header v-on:headerToIndex="onUploadBtnClick"/>
            <ExpansionPanel 
                v-on:cancelUploading="onCancelUploadClick" 
                v-on:cancelAll="onCancelAllClick" 
                v-on:cancelCompleted="onCancelCompletedClick" 
                v-on:dismissIncomplete="onDismissIncompleteClick" 
                v-on:retryIncomplete="onRetryIncompleteClick" 
                :nextUpFiles="nextUpFiles" 
                :uploadingFiles="uploadingFiles" 
                :completedFiles="completedFiles" 
                :inCompletedFiles="inCompletedFiles" 
                :loadingProgress="loadingProgress"
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
        ExpansionPanel
    },

    

    data: () => ({
        uploadingFiles: [],
        nextUpFiles: [],
        inCompletedFiles: [],
        completedFiles: [],
        loadingProgress: 0,
        interval: 0,
    }),

    methods: {
        // Triggered when `headerToIndex` event is emitted by the child.
        onUploadBtnClick () {
            this.uploadingFiles = [
                { type: "fa fa-file-zip-o yellow--text", name: "PhotoID.zip", size: "2.0 MB"},
            ];

            this.nextUpFiles = [
                { type: "fa fa-image green--text", name: "Brithday 2020.png", size: "2.0 MB"},
                { type: "fa fa-file-zip-o yellow--text", name: "Students 2020.zip", size: "2.0 MB"},
                { type: "fa fa-file-pdf-o red--text", name: "License.pdf", size: "2.0 MB"},
                { type: "fa fa-file-excel-o green--text", name: "PayRoll.xlas", size: "2.0 MB"}
            ];

            this.initPrecess();
        },

        initPrecess() {
            this.interval = setInterval(() => {
                console.log(this.loadingProgress);
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
                return;
            }
            
            this.uploadingFiles.push(this.nextUpFiles.shift());
            this.initPrecess();
        },

        onCancelUploadClick () {
            this.loadingProgress = 0;

            this.uploadingFiles.pop();
            
            if (this.nextUpFiles.length == 0)
            {
                alert("end");
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


    }

    
};
</script>
