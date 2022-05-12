<template>
    <div class="pages">
        <div :class="position_class">
            {{page_numbers}}
            <span class="icon-next flipped" :class="{disabled: is_first}" @click="$emit('previous')">
                >
            </span>
            <span class="icon-next" :class="{disabled: is_last}" @click="$emit('next')">
                >
            </span>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        current: {required: true},
        per_page: {required: true},
        total: {required: true},
        relative: {
            default: false,
            type: Boolean
        }
    },
    computed: {
        page_numbers() {
            const initial = ((this.current - 1) * this.per_page) + 1;
            let shown = (this.current) * this.per_page;

            if (shown > this.total) shown = this.total;

            return `${initial}-${shown} of ${this.total}`;
        },
        is_first() {
            return this.current === 1;
        },
        is_last() {
            return this.current * this.per_page >= this.total;
        },
        position_class() {
            return this.relative? 'relative' : 'absolute';
        }
    }
}
</script>

<style lang="scss" scoped>
    .icon-next {
        display: inline-flex;
        font-size: 42px;

        cursor: pointer;
        user-select: none;

        &.flipped {
            transform: scale(-1, 1);
        }
    }

    .pages {
        font-size: 13px;
        color: #6D6D7E;

        display: flex;
        justify-content: center;

        > .relative {
            display: flex;
            align-items: center;
        }

        > .absolute {
            display: flex;
            align-items: center;

            position: absolute;
            right: 30px;
            top: 0;
        }
    }

    .disabled {
        & > .path1::before, > .path2::before {
            filter: opacity(30%);
            cursor: initial;
        }
    }
</style>
