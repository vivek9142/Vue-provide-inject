<template>
  <ul>
    <knowledge-element
      v-for="topic in topics"
      :key="topic.id"
      :id="topic.id"
      :topic-name="topic.title"
      :description="topic.description"
      @select-topic="$emit('select-topic', $event)"
    ></knowledge-element>

    <!-- 04- So this is no longer a pass through component. There's needs and users topics.
        But I also emit select-topic here, and that is still being passed through basically.
        Because ultimately it's the knowledge element. It's this component here,
        which emits select-topic when we click this button. So that's where this custom event 
        is generated,the select-topic event. And then both knowledge grid as well as 
        knowledge base, just forward that custom event to App.vue in the end. So the event 
        gets funneled through to components and topics also gets funneled through at least 
        one component through the knowledge base onto knowledge grid. -->

  </ul>
</template>

<script>
export default {
  props: ['topics'],
  emits: ['select-topic']
};
</script>