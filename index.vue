<!-- DIGF_2008_A3 -->
<!-- SULIM LEE 3177733-->
<template>
    <Page>
        <ActionBar title="DIGF_2008_A3 NativeScript" flat="true" />
        <!-- A Label for your application name -->
        <Label text="DIGF_2008_A3 NativeScript" />
        <StackLayout>
            <!-- Add 4 Tabs (Insert New, Income, Expenses, Messages) -->
            <Tabs>
                <TabStrip>
                    <TabStripItem>
                        <Label text="Insert New"></Label>
                    </TabStripItem>
                    <TabStripItem>
                        <Label text="Income"></Label>
                    </TabStripItem>
                    <TabStripItem>
                        <Label text="Expenses"></Label>
                    </TabStripItem>
                    <TabStripItem>
                        <Label text="Messages"></Label>

                    </TabStripItem>
                </TabStrip>
                <TabContentItem>
                    <ScrollView>
                        <!-- A stacklayout for holding the overall container for
                        your application: this would contain the following: -->
                        <StackLayout class="home-panel">
                            <!-- Add textfields for inserting new Income and
                            Expense Items, and a button to do this in Tab 1 -->
                            <Label textWrap="true" text="Income"
                                class="h2 description-label" />
                            <Label textWrap="true" text="New Income Source"
                                class="h3 description-label" />
                            <TextField v-model="txtIncomeSource"
                                hint="Enter text..." />
                            <Label textWrap="true" text="New Income Amount "
                                class="h3 description-label" />
                            <TextField v-model="txtIncomeAmount"
                                hint="Enter text..." />
                            <Button text="Add New Income" @tap="addIncome" />


                            <Label textWrap="true" text="Expense"
                                class="h2 description-label" />
                            <Label textWrap="true" text="Expense Items Name"
                                class="h3 description-label" />
                            <TextField v-model="txtItem"
                                hint="Enter text..." />
                            <Label textWrap="true" text="Expense Items Cost"
                                class="h3 description-label" />
                            <TextField v-model="txtCost"
                                hint="Enter text..." />
                            <Button text="Add New Expense"
                                @tap="addExpense" />
                        </StackLayout>
                    </ScrollView>
                </TabContentItem>
                <TabContentItem>
                    <GridLayout>
                        <Label text="Income Page" class="h2 text-center">
                        </Label>
                        <!-- Add a listview to display all Income Items in Tab2 -->
                        <ListView for="income in incomes" @itemTap="onItemTap"
                            style="height:1250px">
                            <v-template>
                                <FlexboxLayout flexDirection="row">
                                    <Label :text="income.Source" class="t-12"
                                        style="width: 60%" />
                                    <Label :text="income.Amount" class="t-12"
                                        style="width: 60%" />
                                </FlexboxLayout>
                            </v-template>
                        </ListView>
                    </GridLayout>
                </TabContentItem>
                <TabContentItem>
                    <GridLayout>
                        <Label text="Expenses Page" class="h2 text-center">
                        </Label>
                        <!-- Add a listview to display all Expense Items in Tab 3 -->
                        <ListView for="expense in expenses"
                            @itemTap="onItemTap" style="height:1250px">
                            <v-template>
                                <FlexboxLayout flexDirection="row">
                                    <Label :text="expense.Item" class="t-12"
                                        style="width: 60%" />
                                    <Label :text="expense.Cost" class="t-12"
                                        style="width: 60%" />
                                </FlexboxLayout>
                            </v-template>
                        </ListView>
                    </GridLayout>
                </TabContentItem>
                <TabContentItem>
                    <GridLayout>
                        <ScrollView>
                            <StackLayout class="home-panel">
                                <Label text="Messages Page"
                                    class="h2 text-center">
                                </Label>
                                <!-- Add a running total for showing Total Income,
                                Total Expenses, and Net Income. This will use a
                                set of Labels. -->
                                <!-- Total Expenses -->
                                <Label textWrap="true"
                                    text="You are currently spending this amount!"
                                    class="h3 description-label" />
                                <Label textWrap="true" :text="total"
                                    class="h3 description-label" />

                                <!-- Add conditional rendering for messages to be
                                displayed in Tab 4, depending on whether Total
                                Income, and NetIncome are within
                                the ranges identified in Assignments 1 and 2 -->
                                <Label textWrap="true" v-if="total < 100 "
                                    text="Please spend more!!"
                                    class="h2 description-label" />
                                <Label textWrap="true"
                                    v-if="total >= 100 && total <500"
                                    text="You are doing fine!"
                                    class="h2 description-label" />
                                <Label textWrap="true" v-if="total >=500"
                                    text="You are spending a lot!"
                                    class="h2 description-label" />
                                <!-- Total Income -->
                                <Label textWrap="true"
                                    text="Your Total Income is"
                                    class="h3 description-label" />
                                <Label textWrap="true" :text="totalIncome"
                                    class="h3 description-label" />
                                <!-- Net Income -->
                                <Label textWrap="true"
                                    text="Your Net Income is"
                                    class="h3 description-label" />
                                <Label textWrap="true" :text="netIncome"
                                    class="h3 description-label" />

                                <!-- Another conditional message -->
                                <Label textWrap="true" v-if="netIncome < 0"
                                    text="Your Net income is minus, You are spending too much!!"
                                    class="h2 description-label" />
                                <Label textWrap="true" v-if="netIncome > 0"
                                    text="Your Net income is fine, You are doing fine!"
                                    class="h2 description-label" />

                            </StackLayout>

                        </ScrollView>
                    </GridLayout>
                </TabContentItem>
            </Tabs>
        </StackLayout>
    </Page>
</template>

<script>
    export default {
        methods: {
            onItemTap: function(args) {
                console.log("Item with index: " + args.index + " tapped");
            },

            addExpense() {
                console.log("addExpense Button was pressed");
                var newItem = {
                    Item: this.txtItem,
                    Cost: Number(this.txtCost)
                };
                this.expenses.push(newItem);
                this.total += newItem.Cost;
                this.netIncome = this.totalIncome - this.total;

                // Update, or add logic in your code to show a modal dialog,
                // or popup window, as seen in the tutorial below, this dialog will be shown
                // if Total Income, and NetIncome are within the ranges from Assignment 1 and 2:
                    if (this.total < 100) {
                        alert("Please spend more!!").then(() => {
                            console.log("Please spend more!!");
                        });
                    } else if (this.total >= 100 && this.total < 500) {
                    alert("You are doing fine!").then(() => {
                        console.log("You are doing fine!");
                    });
                } else if (this.total >= 500) {
                    alert("You are spending a lot!").then(() => {
                        console.log("You are spending a lot!");
                    });
                }
            },
            addIncome() {
                console.log("addIncome Button was pressed");
                var newIncome = {
                    Source: this.txtIncomeSource,
                    Amount: Number(this.txtIncomeAmount)
                }; //make a new item
                this.incomes.push(newIncome);
                //append this new item to the inventory list
                this.totalIncome += newIncome.Amount;
                this.netIncome = this.totalIncome - this.total;

                // Update, or add logic in your code to show a modal dialog,
                // or popup window, as seen in the tutorial below, this dialog will be shown
                // if Total Income, and NetIncome are within the ranges from Assignment 1 and 2:
                if (this.netIncome < 0) {
                    alert(
                        "Your Net income is minus, You are spending too much!"
                    ).then(() => {
                        console.log(
                            "Your Net income is minus, You are spending too much!"
                        );
                    });
                } else if (this.netIncome > 0) {
                    alert("Your Net income is fine, You are doing fine").then(
                        () => {
                            console.log(
                                "Your Net income is fine, You are doing fine."
                            );
                        }
                    );
                }
            }
        },

        data() {
            return {
                txtIncomeSource: "",
                txtIncomeAmount: "",
                incomefields: ["Source", "Amount"],
                incomes: [],

                txtItem: "",
                txtCost: "",
                expensefields: ["Item", "Cost"],
                expenses: [],

                //message
                totalIncome: 0,
                netIncome: 0,
                total: 0
            };
        }
    };
</script>

<style scoped>
    .home-panel {
        /* vertical-align: center; */
        font-size: 20;
        margin: 15;
    }

    .description-label {
        margin-bottom: 15;
    }
</style>