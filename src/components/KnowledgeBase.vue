<template>
  <section>
    <h2>Select a Topic</h2>
    <!-- 
      03- I'm forwarding the topics to the knowledge grid, which I'm using here. And I'm 
      basically also just forwarding select-topic. I'm emitting it here in knowledge base, 
      yes. But when do I emit it? Well, I emit it when knowledge-grid emits select-topic.
      
      So basically knowledge base just acts as a pass through component, you could say.
      I pass my topics through to knowledge-grid and I pass select-topic, this event
      through to App.vue when it is emitted by knowledge grid. So knowledge base doesn't 
      really have its own logic.
     -->
    <knowledge-grid :topics="topics" @select-topic="$emit('select-topic', $event)"></knowledge-grid>
  </section>
</template>

<script>
export default {
  props: ['topics'],
  emits: ['select-topic'],
};
</script>