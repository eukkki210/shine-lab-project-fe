<script setup>
// 부모 컴포넌트(MyPage)로부터 전달받은 데이터 정의

const props = defineProps({
    data: { type: Array, default: () => [] },
    loading: { type: Boolean, default: false }
});

</script>

<template>
    <div class="grid">
        <div class="col-12">
            <div class="card">
                <h5>Bottom Grid</h5>
                <DataTable :value="props.data" :loading="props.loading" :paginator="true" :rows="10" dataKey="userId" :rowHover="true" showGridlines>
                    <template #header>
                        <div class="flex justify-content-between flex-column sm:flex-row">
                            <Button type="button" icon="pi pi-filter-slash" label="Clear" outlined @click="clearFilter1()" />
                            <IconField iconPosition="left">
                                <InputIcon class="pi pi-search" />
                                <InputText placeholder="Keyword Search" style="width: 100%" />
                            </IconField>
                        </div>
                    </template>
                    <template #empty> No users found. </template>
                    <template #loading> Loading users data. Please wait. </template>
                    <Column field="userId" header="User ID" style="min-width: 6rem" />
                    <Column field="companyName" header="Company Name" style="min-width: 10rem" />
                    <Column field="userName" header="User Name" style="min-width: 12rem" />
                    <Column field="senior" header="Senior" style="min-width: 12rem" />
                    <Column field="employmentDate" header="Employment Date" style="min-width: 12rem" />
                    <Column field="wage" header="Wage" style="min-width: 12rem" />
                    <Column field="position" header="Position" style="min-width: 12rem" />
                </DataTable>
            </div>
        </div>
    </div>
</template>

<style scoped></style>
