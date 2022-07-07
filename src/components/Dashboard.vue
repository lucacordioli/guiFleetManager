<template>
  <div>
    <h1>LogicMove</h1>
    {{ requestsList }}
  </div>
</template>

<script>


export default {
  name: 'HelloWorld',
  data() {
    return {
      requestsList: [],
      request: {
        pickup_name: null,
        pickup_pos_x: null,
        pickup_pos_y: null,
        dest_name: null,
        dest_pos_x: null,
        dest_pos_y: null,
        slots: null,
        priority: null
      }
    }
  },
  mounted() {
    // eslint-disable-next-line no-undef
    var ros = new ROSLIB.Ros({
      url: 'ws://localhost:9090'
    });
    ros.on('connection', function () {
      console.log('Connected to websocket server.');
    });
    ros.on('error', function (error) {
      console.log('Error connecting to websocket server: ', error);
    });
    ros.on('close', function () {
      console.log('Connection to websocket server closed.');
    });

    // eslint-disable-next-line no-undef
    var listener = new ROSLIB.Topic({
      ros: ros,
      name: '/telegram_request',
      messageType: 'lm_interfaces/msg/Request'
    });
    const that = this
    listener.subscribe(function (message) {
      console.log('Received message on ' + message.pickup_name);
      that.request.pickup_name = message.pickup_name
      that.request.pickup_pos_x = message.pickup_pos_x
      that.request.pickup_pos_y = message.pickup_pos_y
      that.request.dest_name = message.dest_name
      that.request.dest_pos_x = message.dest_pos_x
      that.request.dest_pos_y = message.dest_pos_y
      that.request.slots = message.slots
      that.request.priority = message.priority

      that.requestsList.push(that.request)
      listener.unsubscribe();
    });
  },
  methods: {

  },
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
