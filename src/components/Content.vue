<script>
import helpers from '../helpers/random';
import Icons from './Icons';
import projectsJson from '../data/projects.json';
import workJson from '../data/work.json';
import extrasJson from '../data/extras.json';

export default {
  name: 'Content',
  props: ['flavor', 'time', 'visit', 'weather'],
  components: {
    Icons,
  },
  data() {
    const data = {
      work: workJson,
      projects: projectsJson,
      extras: extrasJson,
      links: {
        queer: [
          'https://www.urbandictionary.com/define.php?term=queer',
          'https://www.huffingtonpost.com/nadia-cho/being-queer-means_b_3510828.html',
          'https://www.uua.org/lgbtq/identity/queer',
        ],
        cisgender: [
          'http://www.transstudent.org/definitions',
          'https://www.healthline.com/health/transgender/what-is-cis',
          'https://www.advocate.com/transgender/2015/07/31/true-meaning-word-cisgender',
        ],
        bubbleTea: [
          'https://www.youtube.com/watch?v=xebewT6lh2k',
          'https://www.youtube.com/watch?v=Ct6BUPvE2sM',
          'https://www.youtube.com/watch?v=LsoLEjrDogU',
        ],
      },
    };

    data.queerLink = helpers.getRandomFromArray(data.links.queer);
    data.cisgenderLink = helpers.getRandomFromArray(data.links.cisgender);
    data.bubbleTeaLink = helpers.getRandomFromArray(data.links.bubbleTea);
    data.landscapePhoto = helpers.getRandomFromArray([
      'landscape-2',
      'landscape-3',
      'landscape-5',
    ]);

    return data;
  },
  computed: {
    accentSectionClasses() {
      const color = (this.time.color.textColor === 'white') ? 'light' : 'dark';
      return [
        `text-${color}`,
      ];
    },
    accentSectionStyles() {
      if (!this.time) {
        return null;
      }

      return {
        backgroundColor: this.time.color.asString,
      };
    },
    featuredProject() {
      return this.projects.find(obj => obj.featured === true);
    },
    landscapePhotoTitle() {
      let title = '';
      if (
        this.landscapePhoto === 'landscape-2'
        || this.landscapePhoto === 'landscape-3'
      ) {
        title = 'Photo I took of the hoodoos at Bryce Canyon in Utah.';
      } else if (this.landscapePhoto === 'landscape-5') {
        title = 'Photo I took of a valley with a river running through it, surrounded by mountains at Zion National Park.';
      }

      return title;
    },
  },
  methods: {
    onQueerClick() {
      this.queerLink = helpers.getRandomFromArray(this.links.queer);
    },
    onCisgenderClick() {
      this.cisgenderLink = helpers.getRandomFromArray(this.links.cisgender);
    },
    onBubbleTeaClick() {
      this.bubbleTeaLink = helpers.getRandomFromArray(this.links.bubbleTea);
    },
  },
};
</script>

<template>
  <div class="content-wrapper">
    <!-- <p class="caption">
        A <a href="https://www.flickr.com/photos/wocintechchat/albums" target="_blank">#WOCinTech Chat stock photo</a>
        of me coding.
    </p> -->
    <section 
      class="photo coding-photo"
      title="my photo">
    </section>

<section class="work">
      <h1>Work History</h1>
      <div 
        class="job" 
        v-for="job in work" 
        v-if="!job.hidden">
        <h3 class="job-header">
          <span class="job-company">{{job.company}}</span>
          <span class="job-duration">{{job.duration}}</span>
        </h3>
        <p class="job-title">{{job.title}}</p>
      <br>
        <p class="job-decs">{{job.desc}}</p>
      </div>
    </section>

    <section>
      <h1>Projects</h1>
      <div 
        class="project" 
        v-for="project in projects" 
        v-if="!project.hidden">
        <h2>
          <a 
            :href="project.link" 
            target="_blank"
            :title="'Link to my '+project.title+' project'">
            {{project.title}}
          </a>
        </h2>
        <p>{{project.description}}</p>
        <p class="project-links">
          <a 
            v-if="!project.hideLinkIcon"
            class="fade-hover"
            :href="project.link"
            target="_blank"
            :title="'Link to my '+project.title+' project'">
            <Icons icon="link" />
          </a>
          <a 
            v-if="project.github"  
            class="fade-hover"
            :href="project.github"
            target="_blank"
            :title="'Github repo link for '+project.title+' project'">
            <Icons icon="code" />
          </a>
          <a 
            v-if="project.dribbble"  
            class="fade-hover"
            :href="project.dribbble"
            target="_blank"
            :title="'Dribbble link for '+project.title+' project'">
            <Icons icon="dribbble" />
          </a>
        </p>
      </div>
    </section>

    <section 
      v-if="featuredProject"
      class="featured-project bg-accent-light"
    >
      <h1>
        Featured Project: 
        <a 
          :href="featuredProject.link" 
          target="_blank"
          :title="'Link to my featured project: '+featuredProject.title">
          {{featuredProject.title}}
        </a>
      </h1>
      <p v-for="paragraph in featuredProject.longDescription" v-html="paragraph"></p>
    </section>
    
    <section 
      class="photo reading-photo"
      title="Me sitting, pretending to read a Python book for a WOCinTech Chat stock photo.">
    </section>
    <p class="caption">
      A <a href="https://www.flickr.com/photos/wocintechchat/albums" target="_blank">#WOCinTech Chat stock photo</a>
      of me reading a Python book.
    </p>

    <section class="about">
      <h1>About</h1>
      <p>
        I’m a <a :href="queerLink" v-on:click="onQueerClick()" target="_blank">queer</a> Trinidadian-American <a :href="cisgenderLink" v-on:click="onCisgenderClick()" target="_blank">cisgender</a> woman of color. 
        I work as a Senior Software Engineer at <a href="https://etsy.com" target="_blank">Etsy</a> in New York, and I’ve been 
        active in the engineering world for over 6 years.
      </p>
      <p>
        I was lucky enough to have a supportive mother, who got me thalida.com 
        for my 16th birthday. It was one of the best gifts I’ve ever received, 
        and is the reason I've become the developer I am today.
      </p>
      <p>
        I now work with an amazing team during the day, and in the wee hours of 
        the night I hack away at one of my many side projects. I'm always 
        looking for a coding buddy, so if you're interested, get in touch! 
        <a href="mailto:hello@thalida.com?subject=Oh%20hi!" target="_blank">hello@thalida.com</a>
      </p>
      <p>
        I’m generally full of questions, and <a :href="bubbleTeaLink" v-on:click="onBubbleTeaClick()" target="_blank">bubble tea</a>.
      </p>
    </section>



    <section class="extras bg-accent-light">
      <h1>In the Wild</h1>
      <div 
        class="extra" 
        v-for="extra in extras" 
        v-if="!extra.hidden">
        <h2><a :href="extra.link" target="_blank">{{extra.title}}</a></h2>
        <p>{{extra.description}}</p>
      </div>
    </section>

    <!-- <p class="caption">
      Photo taken in 2016 during a trip to Utah (Zion National Park/Bryce Canyon)
    </p> -->
    <!-- <section 
      class="photo" 
      :class="landscapePhoto" 
      :title="landscapePhotoTitle">
    </section> -->

    <footer class="footer clearfix">
      <div class="footer-section text-left">
        <span class="footer-credit">
          Powered by <a href="https://darksky.net/poweredby/" target="_blank">DarkSky</a>
        </span>
        <span class="footer-credit">
          View source on <a href="https://github.com/Gustibimo" target="_blank">Github</a>
        </span>
      </div>
    </footer>
  </div>
</template>

<style scoped lang="scss">
  @import '../assets/styles/_variables';

  $padding-base: 32px;

  section {
    margin: 0;
    padding: $padding-base;
    
    @media (min-width: $media-md) and (max-width: $media-lg) { 
      padding: $padding-base * 1.5;
    }
    
    @media (min-width: $media-lg) { 
      padding: $padding-base * 2;
    }
  }

  .bg-accent {
    background-color: $featured-section-bg-color;
  }
  
  .bg-accent-light {
    background-color: rgba($featured-section-bg-color, 0.2);
  }

  .photo {
    display: block;
    padding: 0;
    height: 280px;
    width: 100%;
    background-color: rgba(149,255,232,0.1);
    background-size: cover;
    background-repeat: no-repeat;

    &.etsy-photo {
      background-image: url(../assets/images/etsy-photo.png);
      background-position: center bottom;
    }

    &.reading-photo {
      background-image: url(../assets/images/reading-photo.jpg);
      background-position: center 60%;

      @media (min-width: 500px) { 
        background-position: center 50%;
      }
    }

    &.coding-photo {
      background-image: url(../assets/images/coding-photo.jpg);
      background-position: center center;
    }

    &.landscape-1 {
      background-image: url(../assets/images/landscapes/landscape-1.jpg);
      background-position: center center;
    }

    &.landscape-2 {
      background-image: url(../assets/images/landscapes/landscape-2.jpg);
      background-position: center center;
    }

    &.landscape-3 {
      background-image: url(../assets/images/landscapes/landscape-3.jpg);
      background-position: center center;
    }

    &.landscape-4 {
      background-image: url(../assets/images/landscapes/landscape-4.jpg);
      background-position: center center;
    }

    &.landscape-5 {
      background-image: url(../assets/images/landscapes/landscape-5.jpg);
      background-position: center center;
    }
  }

  .caption {
    padding: 4px 8px;

    background-color: rgba(0, 0, 0, 0.02);

    font-weight: 400;
    font-size: 10px;
    color: rgba($color-dark, 0.50);
    text-align: right;

    a {
      color: rgba($color-dark, 0.70);
    }
  }

  .project,
  .extra {
    padding-top: 16px;
    padding-bottom: 16px;

    &:last-child {
      padding-bottom: 0;
    }

    .project-links {
      margin-top: 8px;
      a {
        margin-right: 8px;
      }
    }
  }

  .featured-project,
  .about {
    p {
      padding-top: 16px;
    }
  }

  .work {
    .job {
      padding-top: 16px;
      padding-bottom: 16px;
    }

    .job-header {
      display: flex;
      flex-direction: row;
      justify-content: space-between;

      padding: 0;
      margin: 0;
      
      font-weight: 400;
      font-size: 16px;
      line-height: 22px;
    }

    .job-company {
      flex: 0 1 auto;
    }

    .job-duration {
      display: none;
      flex: 0 1 auto; 
      text-align: right;
      font-weight: 300;
      font-size: 14px;
    }

    @media (min-width: $media-md) {
      .job-duration {
        display: block;
      }
    }
  }

  .footer {
    display: flex;
    align-items: center;
    padding: ($padding-base / 2);

    .footer-section {
      flex: 0 1 auto;
      width: 50%;
    }
    
    .footer-credit {
      display: block;
      text-transform: uppercase;
      font-size: 10px;
      padding: 4px 0;
    }
    
    @media (min-width: $media-md) and (max-width: $media-lg) { 
      padding-left: $padding-base * 1.5;
      padding-right: $padding-base * 1.5;
    }
    
    @media (min-width: $media-lg) { 
      padding-left: $padding-base * 2;
      padding-right: $padding-base * 2;
    }
  }

</style>
