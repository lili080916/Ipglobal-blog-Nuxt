<template>
    <div>
        <TablePagination
            v-if="show_pagination && !has_pagination_slot"
            :current="current_page"
            :per_page="per_page"
            :total="total_items"
            @previous="page(false)"
            @next="page(true)"/>

        <b-table id="list-table" striped hover
            fixed
            outlined
            :items="items"
            :fields="fields"
            :sort-by.sync="local_sort_by"
            :sort-desc.sync="local_sort_desc"
            :no-provider-paging="true"
            :per-page="per_page"
            :current-page="current_page"
            :busy="loading"
            :no-sort-reset="true"
            @sort-changed="sort"
            style="margin-top: 70px;">

            <template v-if="has_pagination_slot" v-slot:[getPaginationSlot()]>
                <TablePagination
                    v-if="show_pagination"
                    relative
                    :current="current_page"
                    :per_page="per_page"
                    :total="total_items"
                    @previous="page(false)"
                    @next="page(true)"/>
            </template>

            <template #table-busy>
                <Spinner/>
            </template>
            <template
                v-for="slot in Object.keys(this.$scopedSlots)"
                v-slot:[toCellName(slot)]="props"
            >
                <slot v-bind="props" :name="slot" />
            </template>
        </b-table>
    </div>
</template>

<script>
export default {
    data() {
        return {
            current_page: 1,
            local_sort_by: undefined,
            local_sort_desc: undefined
        }
    },
    props: {
        items: {required: true},
        fields: {default: undefined},
        sort_by: {default: undefined},
        sort_desc: {default: undefined},
        per_page: {default: undefined},
        show_pagination: {
            default: false,
            type: Boolean
        },
        loading: {
            default: false,
            type: Boolean
        },
        pagination_slot: {default: undefined}
    },
    created() {
        this.local_sort_by = this.sort_by;
        this.local_sort_desc = this.sort_desc;
    },
    computed: {
        total_items() {
            return this.items.length;
        },
        has_pagination_slot() {
            return this.pagination_slot !== undefined;
        }
    },
    methods: {
        page(next) {
            if (next) {
                if ((this.current_page * this.per_page) < this.total_items) {
                    this.current_page++;
                }
            } else {
                if (this.current_page > 1) {
                    this.current_page--;
                }
            }
        },
        toCellName (slot) {
            return `cell(${slot})`
        },
        getPaginationSlot() {
            return `head(${this.pagination_slot})`;
        },
        sort(ctx) {
            this.$track({
                event: 'click',
                action: 'table_sort',
                additional: {
                    currentPage: ctx.currentPage,
                    perPage: ctx.perPage,
                    sortBy: ctx.sortBy,
                    sortDesc: ctx.sortDesc
                }
            });
        }
    }
}
</script>

<style lang="scss" scoped>
    $pl: 30pxs;

    ::v-deep {
        thead > tr > th, tbody > tr > td {
            &:last-child {
                width: 97px;
            }
        }
        thead > tr > th {
            font-style: normal;
            font-weight: bold;
            font-size: 14px;
            line-height: 19px;

            color: #17334B;

            height: 80px;
            vertical-align: middle;
            user-select: none;

            &:not(:last-child) {
                border-right: 1px solid #DEE2E6;
            }

            &:first-child {
                padding-left: $pl;
            }
        }

        tbody > tr > td {
            font-style: normal;
            font-weight: 600;
            font-size: 13.5px;
            line-height: 100%;

            padding-top: 0 !important;
            padding-bottom: 0 !important;
            height: 48px;

            vertical-align: middle;

            &:not(:last-child) {
                border-right: 1px solid #DEE2E6;
            }

            &:first-child {
                padding-left: $pl;
            }
        }
    }

</style>
