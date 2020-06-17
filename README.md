![message-notification](https://1.bp.blogspot.com/-K8X3ZsT06M4/XupLDTvT03I/AAAAAAAAAkQ/oFKGx6IfKsUCSW1ZKl1T72sebAefExt0wCLcBGAsYHQ/s1600/2020-06-17.png)

## Installation

1. ONLY  FOR   Vuetify UI   !!!!! https://vuetifyjs.com/   thank you Element UI

2. Copy Package `notification` to your VUE project in /src/tools or other  

3. Main.Js  —->>>>     import '@/tools/notification'

4. USE example :      

   ```javascript
   this.$didimessage.error(“something”)		// error message
   this.$didimessage.success(“something”)		// success message
   this.$didimessage.info(“something”)			// info message
   this.$didimessage.warning(“something”)		// warning message
   ```

   

5. Complex usage example : 

   1. ```javascript
      this.$didimessage({
        title: 'This is a title',
        message: 'this is balabalabalabalabalabalabalabalabalabalabalabalabalabalabalabalabalabalabalabala',
        position: 'top-right', // bottom-right // bottom-left // bottom-mid // top-mid
        type: 'success', //type Not required, can be commented out
        duration:6000,  //default timeout is 3s // Not required, can be commented out
        iconClass:'close-network', //Custom icon// Not required, can be commented out 
          // only material desgin https://materialdesignicons.com/
      });
      ```

6. GOOD LUCK , thank you for start! more update www.cakevin.com    //  need google translation