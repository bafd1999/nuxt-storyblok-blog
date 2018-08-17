<template>
  <section id="posts">
    <PostPreview
    v-for="post in posts"
    :key="post.id"
    :title="post.title"
    :excerpt="post.previewText"
    :thumbnailImage="post.thumbnailUrl"
    :id="post.id" />
  </section>
</template>

<script>
import PostPreview from "@/components/Blog/PostPreview";

export default {
  components: {
    PostPreview
  },
  asyncData(context) {
    return context.app.$storyapi
      .get('cdn/stories', {
        version: context.isDev ? 'draft' : 'published',
        starts_with: 'blog/'
      })
      .then(res => {
        return {
          posts: res.data.stories.map(bp => {
            return {
              id: bp.slug,
              title: bp.content.title,
              previewText: bp.content.summary,
              thumbnailUrl: bp.content.thumbnail
            };
          })
        };
      });
  }
  // data() {
  //   return {
  //     posts: [
  //       {
  //       title: 'A New Beginning',
  //       previewText: "This will be awesome, don't miss it!",
  //       thumbnailUrl: 'https://images.unsplash.com/photo-1534353875273-b5887cc1abf5?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=f19b3587a3ced9fadd1abd4716e81212&auto=format&fit=crop&w=2134&q=80',
  //       id: 'a-new-beginning'
  //       },
  //       {
  //       title: 'A Second Beginning',
  //       previewText: "This will be awesome, don't miss it!",
  //       thumbnailUrl: 'https://images.unsplash.com/photo-1534353875273-b5887cc1abf5?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=f19b3587a3ced9fadd1abd4716e81212&auto=format&fit=crop&w=2134&q=80',
  //       id: 'a-second-beginning'
  //       }
  //     ]
  //   }
  // }
}

</script>

<style scoped>
#posts {
  padding-top: 2rem;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

@media (min-width: 35rem) {
  #posts {
    flex-direction: row;
  }
}
</style>
