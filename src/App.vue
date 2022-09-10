So can we use provide inject also for custom events? The answer is yes,
but by implementing that a bit differently. We don't have a similar mechanism for events,
But we can think about this event thing differently. Ultimately, when a custom event is 
emitted, we at some point connect a method here in App dot Vue in this case, 
which should be triggered upon this event. Well, of course, we could also pass such a method
with props down to the component where it should be called. 

So we could remove this SelectTopic custom event here on knowledge base in App dot view.
And we could then go to Knowledge-Base and therefore remove it here as well
and removed is emits option and the same in knowledgeGrid. Remove this listener here on the 
KnowledgeElement and remove this emits option. And in knowledgeElement where we emit this
also not emitted here, but instead do something else. 

Instead here, we could expect to get 
the function that should be called when the button is pressed as props,
or in this case, as an injected value.

<template>
  <div>
    <active-element
      :topic-title="activeTopic && activeTopic.title"
      :text="activeTopic && activeTopic.fullText"
    ></active-element>
    <!-- remove  function-->
    <knowledge-base></knowledge-base>
  </div>
</template>

<script>
export default {
  data() {
    return {
      topics: [
        {
          id: 'basics',
          title: 'The Basics',
          description: 'Core Vue basics you have to know',
          fullText:
            'Vue is a great framework and it has a couple of key concepts: Data binding, events, components and reactivity - that should tell you something!',
        },
        {
          id: 'components',
          title: 'Components',
          description:
            'Components are a core concept for building Vue UIs and apps',
          fullText:
            'With components, you can split logic (and markup) into separate building blocks and then combine those building blocks (and re-use them) to build powerful user interfaces.',
        },
      ],
      activeTopic: null,
    };
  },
  // providing data - PROVIDE method
  provide(){
    return {
      topics:this.topics,
      // attaching select topic function here
      selectTopic:this.activateTopic
    }
  },
  methods: {
    activateTopic(topicId) {
      this.activeTopic = this.topics.find((topic) => topic.id === topicId);
    },
  },
  mounted(){
      setTimeout(()=>{
        this.topics.push({
          id:'events',
          title:'Events',
          description:'Events are important in Vue',
          fullText:'Events allow you to trigger code on demand!'
        })
      },3000)
    }
};
</script>

<style>
* {
  box-sizing: border-box;
}
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
section {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 2rem auto;
  max-width: 40rem;
  padding: 1rem;
  border-radius: 12px;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
}

li {
  border-radius: 12px;
  border: 1px solid #ccc;
  padding: 1rem;
  width: 15rem;
  margin: 0 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

h2 {
  margin: 0.75rem 0;
  text-align: center;
}

button {
  font: inherit;
  border: 1px solid #c70053;
  background-color: #c70053;
  color: white;
  padding: 0.75rem 2rem;
  border-radius: 30px;
  cursor: pointer;
}

button:hover,
button:active {
  background-color: #e24d8b;
  border-color: #e24d8b;
}
</style>