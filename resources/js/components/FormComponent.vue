<template>
    <div class="container">
  
        <div class="row justify-content-center">
            <div class="col-md-6 col-md-offset-3" >

                    
                     <div class="heading" style="padding:20px; font-size:15px;"> <label> Please Submit Your comments </label></div>
                    <div v-if="errors.length">
                         <b>Please correct the following error(s):</b>
                     <div class="box no-border" v-for="error in errors" :key="error.id">
                        <div class="box-tools">
                            <p class="alert alert-danger alert-dismissible" >
                                {{ error }}
                                <button type="button" class="close" data-dismiss="alert" aria-label="Close"><span aria-hidden="true">&times;</span></button>
                            </p>
                        </div>
                    </div>

                    </div>

                    


               
                <div class="panel panel-default">
                   

                    <div class="panel-body">

                      
                        

                <form action="./api/comments" method="POST" enctype="multipart/form-data" @submit.prevent="OnSubmit">

                         
                     
                        <div class="form-group">
                            <label for="name">Comments:<span class="text-danger">*</span></label>
                            <textarea 
                                id="comments"
                                class="form-control"
                                v-model="userData.comments">  </textarea>
                        </div>

                         <div class="form-group">
                            <label for="name">Your Name:<span class="text-danger">*</span></label>
                            <input type="text"
                                id="name"
                                class="form-control"
                                v-model="userData.name">
                        </div>
                        <div class="form-group">
                            <label for="email">Your Email Address:<span class="text-danger">*</span></label>
                            <input type="email"
                                id="email"
                                class="form-control"
                                v-model="userData.email">
                        </div>

                         <div class="form-group">
                            <label for="phone_no">Your Phone_no:<span class="text-danger">*</span></label>
                            <input type="number"
                                id="phone_no"
                                class="form-control"
                                v-model="userData.phone_no">
                        </div>

                        <div class="panel-footer">
                        <button  type="submit" class="btn btn-primary pull-right" > Submit Form</button>
                         
                         </div>
                          
                    </form>
                    </div>
                    
                </div>
                <button type="button" class="btn btn-success" @click="list()">View All comments</button>
                     
                <div class="row" v-if="allComments.length">
                   <div class="col-md-12" style="padding-top:10px;">
                      <div class="panel-group">
                        <div class="panel panel-default"  v-for="comment in allComments" :key="comment.id">
                        <div class="panel-heading">{{ comment.name}} ({{comment.email}})</div>
                        <div class="panel-body">{{ comment.comment}}</div>
                        </div>
                       
                    </div>
                </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                userData: {
                    email: '',
                    name: '',
                    phone_no: '',
                    comments: '',
                   
                     
                            },
                     errors: [],
                     allComments: [],
                     message:"nothing",
                    
                    }
                 },
        
                 methods: {

                      list() {
                       
                         window.axios.get('/api/comments').then(({ data }) => {
                             
                         this.allComments = data;
                            
                            });

                    },
                    
                    OnSubmit(){
                                if(this.userData.email && this.userData.name && this.userData.phone_no && this.userData.comments) {
                                axios.post('/api/comments', {
                                     email : this.userData.email,
                                    name : this.userData.name,
                                    phone_no : this.userData.phone_no,
                                    comment : this.userData.comments,
                                })
                                .then(function (response) {
                                   
                                    console.log(response);
                                })
                                .catch(function (error) {
                                   
                                    console.log(error);
                                });

                                 this.userData.email = "";
                                 this.userData.name = "";
                                 this.userData.phone_no = "";
                                 this.userData.comments = "";
                               
                                  }

                                 else{

                                   if(!this.userData.email) this.errors.push("Email required.")
                                    if(!this.userData.name) this.errors.push("Name required.")
                                    if(!this.userData.phone_no) this.errors.push("phone_no required.")
                                    if(!this.userData.comments) this.errors.push("comment required.")
                                 }

                    },

                 }

    }



</script>
