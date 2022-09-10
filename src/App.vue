we need to topic steer. and we definitely, also need to manage them in App.vue.
Since App.vue, is the place where active element, which needs to know about the topics, and 
KnowledgeBase, and therefor, ultimately KnowledgeGrid are connected. So how can we pass 
the topics to KnowledgeGrid, which needs to topics, if we don't want to pass them through 
KnowledgeBase?

let's explore another feature Vue offers us, and that is, provide and inject,
a pattern you can use to provide data in one place and inject it,
which means use it, in another place.

Now here in App.vue, we could, for example, provide our topics here.
We do this by adding a provide option to our config object, anywhere in the conflict object.
Here I'm adding it between data and methods. And provide can hold an object then,
in which you, for example, provide your topics. So here, I could then grab this topics array,
and provide it down there. So now I'm providing this topics array. Now this is step one.
Now we're providing this data, 

We also need to listen to that provided data. We need to use that provided data somewhere,
and that in our case, which take place in the KnowledgeGrid because that is where we need the 
topics. 

For that here, rather than expecting to get topics as props, we add another option, 
the inject option. Which works together with provide. Inject, basically works like props, 
you provide an array, and now you reference all the provided data you wanna use in this 
component. So at the moment, there's just one piece of data we are providing, and that would 
be topics.

We're providing that in App.vue, under the key topics here. So we can use that key, and 
inject here in KnowledgeGrid. And here's one important note, you can only inject what has 
been provided on a higher-up level. Which basically means, in a parent component,
or an ancestor component of KnowledgeGrid.

<template>
  <div>
    <active-element
      :topic-title="activeTopic && activeTopic.title"
      :text="activeTopic && activeTopic.fullText"
    ></active-element>

    <knowledge-base :topics="topics" @select-topic="activateTopic"></knowledge-base>
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
  // providing data - PROVIDE

  /*
  However, there is a problem, we're now basically, creating this array twice.
  We have it here, in App.vue in our data, and we then have it again, in the provide object.
  This works in this case, but it is code duplication. And if we had any logic in this app 
  to change this data stored array, this change would not be reflected in the provided data,
  because that's a brand new array. It's a brand new object in memory there for.
  So if we ever changed his original array here in data, such changes would not be passed on 
  to components that rely on the provided topics.it turns out, we can do that for that,
  for that we need to change the way we use provide though. Instead of setting it equal to 
  an object with the provided data, we now convert provide to a method, so now to provide 
  option holds a function in the end. And in there we return our provided object, 
  so it now basically, works a little bit like data. 
  
  it turns out, we can do that for that, for that we need to change the way we use provide 
  though. Instead of setting it equal to an object with the provided data, we now convert 
  provide to a method, so now to provide option holds a function in the end. And in there we 
  return our provided object, so it now basically, works a little bit like data.
  */ 
  // provide:{
  //   topics:[
  //       {
  //         id: 'basics',
  //         title: 'The Basics',
  //         description: 'Core Vue basics you have to know',
  //         fullText:
  //           'Vue is a great framework and it has a couple of key concepts: Data binding, events, components and reactivity - that should tell you something!',
  //       },
  //       {
  //         id: 'components',
  //         title: 'Components',
  //         description:
  //           'Components are a core concept for building Vue UIs and apps',
  //         fullText:
  //           'With components, you can split logic (and markup) into separate building blocks and then combine those building blocks (and re-use them) to build powerful user interfaces.',
  //       },
  //     ],
  // },

  provide(){
    return {
      topics:this.topics
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