<script setup>
// 부모 컴포넌트(MyPage)로부터 전달받은 데이터(data.json) 정의
import { ref } from 'vue';

const props = defineProps({
    data: {
        type: Array,
        default: () => []
    }
});

const searchQuery = ref({
    companyId: null,
    companyName: null,
    country: null
});

// 특정 행이 클릭되었을 때 발생하는 이벤트 정의
const emits = defineEmits(['rowClick', 'search']);

const onRowSelect = (row) => {
    emits('rowClick', row);
};

const searchCompanies = () => {
    console.log('검색 버튼 클릭: ', searchQuery.value);
    emits('search', searchQuery.value);
};
</script>

<template>
    <div class="grid">
        <div class="col-12">
            <div class="card">
                <h5>Top Grid</h5>
                <div class="col-12 mb-2 lg:col-6 lg:mb-0">
                    <div class="p-inputgroup">
                        <span class="p-inputgroup-addon">
                            <i class="pi pi-id-card"></i>
                        </span>
                        <InputText v-model="searchQuery.companyId" placeholder="회사 ID" class="p-inputtext" />
                        <span class="p-inputgroup-addon">
                            <i class="pi pi-building"></i>
                        </span>
                        <InputText v-model="searchQuery.companyName" placeholder="회사 이름" class="p-inputtext" />
                        <span class="p-inputgroup-addon">
                            <i class="pi pi-globe"></i>
                        </span>
                        <InputText v-model="searchQuery.country" placeholder="국가" class="p-inputtext" />
                        <button class="p-button p-button-primary" @click="searchCompanies">검색</button>
                    </div>
                </div>
                <DataTable :value="props.data" selectionMode="single" @rowClick="onRowSelect" :paginator="true" :rows="10" dataKey="id" :rowHover="true" showGridlines>
                    <template #empty> No companies found. </template>
                    <template #loading> Loading companies data. Please wait. </template>
                    <Column field="companyId" header="ID" style="min-width: 1rem" />
                    <Column field="companyName" header="Name" style="min-width: 12rem" />
                    <Column field="country" header="Country" style="min-width: 12rem" />
                </DataTable>
            </div>
        </div>
    </div>
</template>

<style scoped lang="scss">
:deep(.p-datatable-frozen-tbody) {
    font-weight: bold;
}

:deep(.p-datatable-scrollable .p-frozen-column) {
    font-weight: bold;
}
</style>
