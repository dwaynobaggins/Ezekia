<template>
    <div class="museum-highlight" >
        <img class="museum-highlight__image fadeIn" :src="imageRandom" alt="Dwayne Brick">
        <div class="museum-highlight__contents" :class="bgIsPartner">
            <div class="museum-highlight__contents__title__container flex flex--justify flex--align-items-center">
                <h2 class="museum-highlight__contents__title__container__title" :class="titleIsPartner"> {{ highlight.name }} </h2>
                <p class="museum-highlight__contents__title__container__date"> {{ highlightDate }} </p>
            </div>
            <p> {{ highlight.description }} </p>
            <div v-if="highlight.news" class="museum-highlight__contents__news">                
                <div class="flex flex--justify flex--align-items-center">
                    <h3 class="museum-highlight__contents__news__heading">News</h3>
                    <!-- the time seems more relevant for the news section so I've left it in -->
                    <p class="museum-highlight__contents__news__date" v-if="highlight.date"> {{ highlight.news.date }}</p>
                </div>
                <p class="museum-highlight__contents__news__title"> {{ highlight.news.title }}</p>                
            </div>
            <div v-if="type === 'Space'">
                <p class="museum-highlight__contents__quiz" v-if="highlight.quiz">Click here to try our <a :href="highlight.quiz">Quiz!</a></p>
            </div>
            <div v-else-if="type === 'Dinosaurs'">
                <!-- code block for extra unique Dinosaur data would go here, etc -->
            </div>            
            <div class="flex flex--col flex--align-items-center">
                <button class="museum-highlight__contents__btn" @click="refresh">Refresh</button>
            </div>            
            <div class="museum-highlight__contents__badge">
                <!-- for loading in badge a slot was used so the badge could take any form -->
                <slot></slot>
            </div>
        </div>
    </div>
    
</template>

<script>

export default {
    name: 'MuseumHighlight',
    inject: ['accumulatedPictureCount'],
    props: {
        highlight: { type: Object },
        index: { type: Number, default: 0 },
        type: { type: String, default: "space" },
    },
    data() {
        return {
            pictureCount: 0,
        };
    },
    computed: {
        imageRandom() {
            return `https://picsum.photos/300/200?random=${this.pictureCount}`;
        },
        highlightDate() {
            // Changes the highlight date into a more user friendly format
            const date = this.highlight.date.split(" ");
            const event = new Date(Date.UTC(...date[0].split('-')));
            const localDateString = event.toLocaleDateString('en-GB', {
                year: 'numeric',
                month: 'long',
                day: 'numeric',
            })

            const indexOfFirstSpace = localDateString.indexOf(" ");
            return this.dateOrdinal(localDateString.slice(0, indexOfFirstSpace)) + localDateString.slice(indexOfFirstSpace);
        },
        bgIsPartner() {
            return this.highlight.isPartner ? "museum-highlight__contents--is-partner" : "";
        },
        titleIsPartner() {
            return this.highlight.isPartner ? "museum-highlight__contents__title__container__title--is-partner" : "";
        }        
    },
    methods: {
        dateOrdinal(d) {
            return d+(31==d||21==d||1==d?"st":22==d||2==d?"nd":23==d||3==d?"rd":"th");
        },

        refresh() {
            this.$emit('incrementPictureCount');
            this.pictureCount = this.accumulatedPictureCount;
        }
    },
    created() {
        this.pictureCount = this.index;
    },
};
</script>

<style lang="scss" scoped>

.museum-highlight {
    max-width: 300px;
    width: 100%;
    position: relative;

    &__image {
        width: 100%;
        display: flex;
        opacity: 0;
        min-height: 200px;
    }

    &__contents {
        padding: 8px;

        &--is-partner {
            background: linear-gradient(0deg, rgba(44,55,145,0.5) 0%, rgba(255,255,255,1) 69%);
        }

        &__title__container {
            gap: 8px;
            padding: 8px 0;

            &__title--is-partner {
                color: var(--eze-special-blue);
            }

            &__title__date {
                font-size: 14px;
                font-weight: 600;
                text-align: end;
            }   
        }        

        &__news {
            margin-top: 8px;
            border-radius: 8px;
            border: var(--eze-blue) solid 1px;
            padding: 8px;

            &__news__heading {
                font-style: italic;
            }

            &__news__title {
                font-weight: 500;
            }
        }

        &__quiz {
            margin-top: 8px;
        }

        &__btn {
            margin: 16px 0;
            background-color: var(--eze-blue);
            border: 1px solid var(--eze-blue);
            border-radius: 6px;
            box-shadow: 0 0 5px #212633ad;
            color: #fff;
            cursor: pointer;
            display: inline-flex;
            width: max-content;
            font-weight: 500;
            padding: 14px 25px;
            font-size: 16px;
            box-shadow: 2px 0 7px #212633ad;
        }

        &__btn:hover,
        &__btn:focus {
            transform: scale(1.02);
        }

        &__badge {
            position: absolute;
            top: -25px;
            right: -25px;
        }
    }    
}

</style>





