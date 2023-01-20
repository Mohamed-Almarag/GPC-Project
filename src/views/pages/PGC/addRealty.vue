<template>
  <div class="main_ecb_wrapper">
    <!-- //// Start Project Details  -->
    <b-row>
      <b-col>
        <div align="right">
          <b-row class="bg-white pt-2 pb-2">
            <div class="container">
              <!-- //! Start Part One In Form Details  -->
              <!-- بيانات المشروع  //// Start Project Details-->
              <b-overlay
                v-if="show_model_inputs == 1"
                variant="white"
                spinner-variant="primary"
                blur="0"
                opacity=".75"
                rounded="sm"
              >
                <div class="add_project_details_wrapper">
                  <validation-observer ref="projectDetailsForm">
                    <b-form
                      @submit.prevent="submitForm('projectDetailsForm', 2)"
                    >
                      <b-row class="bg-white pt-2 pb-2">
                        <b-col md="12" class="back_ground">
                          <p class="text-center">بيانات المشروع</p>
                        </b-col>
                        <!-- Project Name  -->
                        <b-col md="6">
                          <AppInput
                            type="text"
                            placeholder="اسم المشروع"
                            label="اسم المشروع"
                            v-model="form.submission.pro_name"
                          ></AppInput>
                        </b-col>
                        <!-- Project Number  -->
                        <b-col md="6">
                          <AppInput
                            type="number"
                            label="رقم المشروع"
                            placeholder="رقم المشروع"
                            v-model="form.submission.pro_num"
                          ></AppInput>
                        </b-col>
                        <!-- Project Zone  -->
                        <b-col md="6">
                          <AppInput
                            type="number"
                            label="رقم المنطقة"
                            placeholder="رقم المنطقة"
                            v-model="form.submission.zone"
                          ></AppInput>
                        </b-col>
                        <!-- Project Zone  -->
                        <b-col md="6">
                          <AppInput
                            type="number"
                            label="رقم اللوحة"
                            placeholder="رقم اللوحة"
                            v-model="form.submission.plad_num"
                          ></AppInput>
                        </b-col>

                        <b-col cols="2 text-right pt-2">
                          <b-button variant="info" type="submit">
                            التالي
                          </b-button>
                        </b-col>
                      </b-row>
                    </b-form>
                  </validation-observer>
                </div>
              </b-overlay>
              <!-- بيانات المشروع  //// End Project Details-->

              <!--//? بيانات المالك  Start Owner Details -->
              <b-overlay
                v-if="show_model_inputs == 2"
                variant="white"
                spinner-variant="primary"
                blur="0"
                opacity=".75"
                rounded="sm"
              >
                <div class="add_project_details_wrapper">
                  <validation-observer ref="ownerDetailsForm">
                    <b-form @submit.prevent="submitForm('ownerDetailsForm', 3)">
                      <b-row class="bg-white pt-2 pb-2">
                        <b-col md="12" class="back_ground">
                          <p class="text-center">بيانات المالك</p>
                        </b-col>
                      </b-row>
                      <b-form-group
                        class="text-right"
                        v-if="ownersFormLenght() == 0"
                      >
                        <b-button @click="addOwner"> اضف</b-button>
                      </b-form-group>
                      <b-row v-for="(owner, i) in form.owners" :key="i">
                        <!-- Owner Name -->
                        <b-col md="3">
                          <AppInput
                            type="text"
                            label="اسم المالك"
                            placeholder="اسم المالك"
                            v-model="owner.name"
                          ></AppInput>
                        </b-col>
                        <!-- Phone Number -->
                        <b-col md="3">
                          <AppInput
                            type="number"
                            label="رقم الجوال"
                            placeholder="رقم الجوال"
                            v-model="owner.phone"
                          ></AppInput>
                        </b-col>
                        <!-- National Number -->
                        <b-col md="2">
                          <AppInput
                            type="number"
                            label="رقم الهوية"
                            placeholder="رقم الهوية"
                            v-model="owner.national_id"
                          ></AppInput>
                        </b-col>
                        <!-- National Number -->
                        <b-col md="2">
                          <b-form-group class="text-right" label="نوع الهوية">
                            <validation-provider
                              #default="{ errors }"
                              name=" نوع الهوية"
                              rules="required"
                            >
                              <v-select
                                placeholder="نوع الهوية"
                                :options="Array.from(personality, (el) => el)"
                                :dir="
                                  $store.state.appConfig.layout.isRTL
                                    ? 'rtl'
                                    : 'ltr'
                                "
                                v-model="owner.id_type"
                                :reduce="(val) => val"
                              >
                              </v-select>
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                        <b-col md="2" class="d-flex align-items-center">
                          <div class="d-flex align-items-center">
                            <b-button @click="addOwner" class="ml-2">
                              اضف</b-button
                            >
                            <b-button @click="form.owners.pop()">حذف</b-button>
                          </div>
                        </b-col>
                      </b-row>
                      <div class="d-flex justify-content-between mt-4">
                        <b-button variant="primary" @click="show_model(1)">
                          السابق
                        </b-button>
                        <!-- @click="show_model(3)" -->
                        <b-button variant="info" type="submit">
                          التالي
                        </b-button>
                      </div>
                    </b-form>
                  </validation-observer>
                </div>
              </b-overlay>
              <!--//? بيانات المالك  End Owner Details -->

              <!-- //// بيانات الصك Start Contract Details -->
              <b-overlay
                v-if="show_model_inputs == 3"
                variant="white"
                spinner-variant="primary"
                blur="0"
                opacity=".75"
                rounded="sm"
              >
                <div class="add_project_details_wrapper">
                  <validation-observer ref="contractDetailsForm">
                    <b-form
                      @submit.prevent="submitForm('contractDetailsForm', 4)"
                    >
                      <b-row class="bg-white pt-2 pb-2">
                        <b-col md="12" class="back_ground">
                          <p class="text-center">بيانات الصك</p>
                        </b-col>
                        <!-- Contract Number -->
                        <b-col md="6">
                          <AppInput
                            type="number"
                            label="رقم الصك"
                            placeholder="رقم الصك"
                            v-model="form.submission.contract_number"
                          ></AppInput>
                        </b-col>
                        <!-- Contract Dtae -->
                        <b-col md="6">
                          <b-form-group class="text-right" label="تاريخ الصك">
                            <validation-provider
                              #default="{ errors }"
                              name="تاريخ الصك"
                              rules="required"
                            >
                              <b-form-datepicker
                                v-model="form.submission.contract_date"
                                :state="errors.length > 0 ? false : null"
                                label-no-date-selected="تاريخ الصك"
                              />
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                        <!-- Contract Source -->
                        <b-col md="6">
                          <AppInput
                            type="text"
                            label="مصدر الصك"
                            placeholder="مصدر الصك"
                            v-model="form.submission.contract_source"
                          ></AppInput>
                        </b-col>
                        <!-- Contract Area -->
                        <b-col md="6">
                          <AppInput
                            type="number"
                            label="المساحة حسب الصك"
                            placeholder="المساحة حسب الصك"
                            v-model="form.submission.contract_area"
                          ></AppInput>
                        </b-col>
                      </b-row>
                      <div class="d-flex justify-content-between mt-4">
                        <b-button variant="primary" @click="show_model(2)">
                          السابق
                        </b-button>
                        <!-- @click="show_model(3)" -->
                        <b-button variant="info" type="submit">
                          التالي
                        </b-button>
                      </div>
                    </b-form>
                  </validation-observer>
                </div>
              </b-overlay>
              <!-- //// بيانات الصك End Contract Details -->

              <!-- //? حدود العقار  بالصك Start Contract Border Details -->
              <b-overlay
                v-if="show_model_inputs == 4"
                variant="white"
                spinner-variant="primary"
                blur="0"
                opacity=".75"
                rounded="sm"
              >
                <div class="add_project_details_wrapper">
                  <validation-observer ref="contractBorderDetailsForm">
                    <b-form
                      @submit.prevent="
                        submitForm('contractBorderDetailsForm', 5)
                      "
                    >
                      <b-row class="bg-white pt-2 pb-2">
                        <b-col md="12" class="back_ground">
                          <p class="text-center">
                            الحدود والأطوال بموجب وثيقة الملكية
                          </p>
                        </b-col>
                        <!-- Contract North Direction -->
                        <b-col md="6">
                          <AppInput
                            type="number"
                            label="إتجاه الشمال"
                            placeholder="إتجاه الشمال"
                            v-model="
                              form.submission.contract_border_details.north_dir
                            "
                          ></AppInput>
                        </b-col>
                        <!-- Contract North Length -->
                        <b-col md="6">
                          <AppInput
                            type="number"
                            label="طول الشمال"
                            placeholder="طول الشمال"
                            v-model="
                              form.submission.contract_border_details
                                .north_length
                            "
                          ></AppInput>
                        </b-col>
                        <!-- Contract South Direction -->
                        <b-col md="6">
                          <AppInput
                            type="number"
                            label="إتجاه الجنوب"
                            placeholder="إتجاه الجنوب"
                            v-model="
                              form.submission.contract_border_details.south_dir
                            "
                          ></AppInput>
                        </b-col>
                        <!-- Contract South Length -->
                        <b-col md="6">
                          <AppInput
                            type="number"
                            label="طول الجنوب"
                            placeholder="طول الجنوب"
                            v-model="
                              form.submission.contract_border_details
                                .south_length
                            "
                          ></AppInput>
                        </b-col>
                        <!-- Contract East Direction -->
                        <b-col md="6">
                          <AppInput
                            type="number"
                            label="إتجاه الشرق"
                            placeholder="إتجاه الشرق"
                            v-model="
                              form.submission.contract_border_details.east_dir
                            "
                          ></AppInput>
                        </b-col>
                        <!-- Contract East Length -->
                        <b-col md="6">
                          <AppInput
                            type="number"
                            label="طول الشرق"
                            placeholder="طول الشرق"
                            v-model="
                              form.submission.contract_border_details
                                .east_length
                            "
                          ></AppInput>
                        </b-col>
                        <!-- Contract West Direction -->
                        <b-col md="6">
                          <AppInput
                            type="number"
                            label="إتجاه الغرب"
                            placeholder="إتجاه الغرب"
                            v-model="
                              form.submission.contract_border_details.west_dir
                            "
                          ></AppInput>
                        </b-col>
                        <!-- Contract West Length -->
                        <b-col md="6">
                          <AppInput
                            type="number"
                            label="طول الغرب"
                            placeholder="طول الغرب"
                            v-model="
                              form.submission.contract_border_details
                                .west_length
                            "
                          ></AppInput>
                        </b-col>
                      </b-row>
                      <div class="d-flex justify-content-between mt-4">
                        <b-button variant="primary" @click="show_model(3)">
                          السابق
                        </b-button>
                        <!-- @click="show_model(3)" -->
                        <b-button variant="info" type="submit">
                          التالي
                        </b-button>
                      </div>
                    </b-form>
                  </validation-observer>
                </div>
              </b-overlay>
              <!-- //? حدود العقار  بالصك End Contract Border Details -->

              <!-- //! End Part One In Form Details  -->

              <!-- بيانات العقار  -->
              <b-overlay
                v-if="show_model_inputs == 5"
                variant="white"
                spinner-variant="primary"
                blur="0"
                opacity=".75"
                rounded="sm"
              >
                <div class="add_project_details_wrapper">
                  <validation-observer ref="addProjectRules">
                    <b-form v-if="this.hide == true">
                      <b-row class="bg-white pt-2 pb-2">
                        <b-col md="12" class="back_ground">
                          <p class="text-center">بيانات العقار</p>
                        </b-col>

                        <b-col md="1"></b-col>

                        <b-col class="d-flex justify-content-center" md="8">
                        </b-col>
                        <b-col md="4">
                          <b-form-group class="text-right" label="رقم العقار">
                            <validation-provider
                              #default="{ errors }"
                              name="رقم العقار"
                              rules="required"
                            >
                              <b-form-input
                                v-model="form.submission.building_number"
                                :state="errors.length > 0 ? false : null"
                                placeholder="رقم العقار"
                                type="text"
                              />
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                        <b-col md="4">
                          <b-form-group class="text-right" label="رقم المخطط">
                            <validation-provider
                              #default="{ errors }"
                              name="رقم المخطط"
                              rules="required"
                            >
                              <b-form-input
                                v-model="form.submission.planned_num"
                                :state="errors.length > 0 ? false : null"
                                placeholder="رقم المخطط"
                                type="text"
                              />
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                        <b-col md="4">
                          <b-form-group class="text-right" label="نوع العقار ">
                            <validation-provider
                              #default="{ errors }"
                              name="نوع العقار"
                              rules="required"
                            >
                              <v-select
                                placeholder="نوع العقار"
                                :options="
                                  Array.from(submissionTypes, (el) => el)
                                "
                                :dir="
                                  $store.state.appConfig.layout.isRTL
                                    ? 'rtl'
                                    : 'ltr'
                                "
                                v-model="form.submission.building_type"
                                :reduce="(val) => val"
                              >
                              </v-select>
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                        <b-col md="4">
                          <b-form-group
                            class="text-right"
                            label=" نوع الملكية "
                          >
                            <validation-provider
                              #default="{ errors }"
                              name=" نوع الملكية"
                              rules="required"
                            >
                              <v-select
                                placeholder="نوع الملكية"
                                :options="Array.from(preportyTypes, (el) => el)"
                                :dir="
                                  $store.state.appConfig.layout.isRTL
                                    ? 'rtl'
                                    : 'ltr'
                                "
                                v-model="form.submission.contract_type"
                                :reduce="(val) => val"
                              >
                              </v-select>
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                        <b-col md="4">
                          <b-form-group class="text-right" label="الاستخدام">
                            <validation-provider
                              #default="{ errors }"
                              name="الاستخدام"
                              rules="required"
                            >
                              <b-form-input
                                v-model="form.submission.submission_using"
                                :state="errors.length > 0 ? false : null"
                                placeholder="الاستخدام"
                              />
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                        <!-- <b-col md="4">
                                                        <b-form-group class="text-right" label="وصف العقار ">
                                                            <validation-provider #default="{ errors }" name="وصف العقار"
                                                                rules="required">
                                                                <v-select
                                                                    placeholder="وصف العقار"
                                                                    :options="Array.from(submissionDesc , (el) => el)"
                                                                    :dir="$store.state.appConfig.layout.isRTL ? 'rtl': 'ltr' "
                                                                    v-model="form.submission.submission_desc"
                                                                    :reduce="(val) => val"
                                                                >
                                                                </v-select>
                                                                <small class="text-danger" v-if="errors[0]">هذا الحقل
                                                                    مطلوب</small>
                                                            </validation-provider>
                                                        </b-form-group>
                                                    </b-col> -->
                        <b-col md="4">
                          <b-form-group
                            class="text-right"
                            label=" مساحة العقار م2"
                          >
                            <validation-provider
                              #default="{ errors }"
                              name="مساحة العقار م2"
                              rules="required"
                            >
                              <b-form-input
                                v-model="form.submission.submission_area"
                                :state="errors.length > 0 ? false : null"
                                placeholder="مساحة العقار م2"
                                type="number"
                              />
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                      </b-row>
                      <b-row>
                        <b-col cols="2 text-right pt-2">
                          <b-button variant="primary" @click="show_model(4)">
                            السابق
                          </b-button>
                        </b-col>

                        <b-col cols="8 text-right pt-2"> </b-col>
                        <b-col cols="2 text-right pt-2">
                          <b-button variant="info" @click="show_model(6)">
                            التالي
                          </b-button>
                        </b-col>
                      </b-row>
                    </b-form>
                  </validation-observer>
                </div>
              </b-overlay>
              <!-- حدود العقار -->
              <b-overlay
                v-if="show_model_inputs == 6"
                variant="white"
                spinner-variant="primary"
                blur="0"
                opacity=".75"
                rounded="sm"
              >
                <div class="add_project_details_wrapper">
                  <validation-observer ref="addProjectRules">
                    <b-form v-if="this.hide == true">
                      <b-row class="bg-white pt-2 pb-2">
                        <b-col md="12" class="back_ground">
                          <p class="text-center">
                            الحدود والأطوال بموجب أعمال الحصر الميداني
                          </p>
                        </b-col>

                        <b-col md="1"></b-col>

                        <b-col class="d-flex justify-content-center" md="8">
                        </b-col>
                        <b-col md="6">
                          <b-form-group class="text-right" label="أتجاه الشمال">
                            <validation-provider
                              #default="{ errors }"
                              name="أتجاه الشمال"
                              rules="required"
                            >
                              <b-form-input
                                v-model="
                                  form.submission.restrict_border.north_dir
                                "
                                :state="errors.length > 0 ? false : null"
                                placeholder="أتجاه الشمال"
                                type="text"
                              />
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                        <b-col md="6">
                          <b-form-group class="text-right" label="طول الشمال ">
                            <validation-provider
                              #default="{ errors }"
                              name="طول الشمال"
                              rules="required"
                            >
                              <b-form-input
                                v-model="
                                  form.submission.restrict_border.north_length
                                "
                                :state="errors.length > 0 ? false : null"
                                placeholder="أتجاه الشمال"
                                type="number"
                              />
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                        <b-col md="6">
                          <b-form-group class="text-right" label="أتجاه الجنوب">
                            <validation-provider
                              #default="{ errors }"
                              name="أتجاه الجنوب"
                              rules="required"
                            >
                              <b-form-input
                                v-model="
                                  form.submission.restrict_border.south_dir
                                "
                                :state="errors.length > 0 ? false : null"
                                placeholder="أتجاه الجنوب"
                                type="text"
                              />
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                        <b-col md="6">
                          <b-form-group class="text-right" label="طول الجنوب ">
                            <validation-provider
                              #default="{ errors }"
                              name="طول الجنوب"
                              rules="required"
                            >
                              <b-form-input
                                v-model="
                                  form.submission.restrict_border.south_length
                                "
                                :state="errors.length > 0 ? false : null"
                                placeholder="طول الجنوب"
                                type="number"
                              />
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                        <b-col md="6">
                          <b-form-group class="text-right" label="أتجاه الشرق">
                            <validation-provider
                              #default="{ errors }"
                              name="أتجاه الشرق"
                              rules="required"
                            >
                              <b-form-input
                                v-model="
                                  form.submission.restrict_border.east_dir
                                "
                                :state="errors.length > 0 ? false : null"
                                placeholder="أتجاه الشرق"
                                type="text"
                              />
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                        <b-col md="6">
                          <b-form-group class="text-right" label="طول الشرق ">
                            <validation-provider
                              #default="{ errors }"
                              name="طول الشرق"
                              rules="required"
                            >
                              <b-form-input
                                v-model="
                                  form.submission.restrict_border.east_length
                                "
                                :state="errors.length > 0 ? false : null"
                                placeholder="طول الشرق"
                                type="number"
                              />
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                        <b-col md="6">
                          <b-form-group class="text-right" label="أتجاه الغرب">
                            <validation-provider
                              #default="{ errors }"
                              name="أتجاه الغرب"
                              rules="required"
                            >
                              <b-form-input
                                v-model="
                                  form.submission.restrict_border.west_dir
                                "
                                :state="errors.length > 0 ? false : null"
                                placeholder="أتجاه الغرب"
                                type="text"
                              />
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                        <b-col md="6">
                          <b-form-group class="text-right" label="طول الغرب ">
                            <validation-provider
                              #default="{ errors }"
                              name="طول الغرب"
                              rules="required"
                            >
                              <b-form-input
                                v-model="
                                  form.submission.restrict_border.west_length
                                "
                                :state="errors.length > 0 ? false : null"
                                placeholder="طول الغرب"
                                type="number"
                              />
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                      </b-row>
                      <b-row>
                        <b-col cols="2 text-right pt-2">
                          <b-button variant="primary" @click="show_model(5)">
                            السابق
                          </b-button>
                        </b-col>

                        <b-col cols="8 text-right pt-2"> </b-col>
                        <b-col cols="2 text-right pt-2">
                          <b-button variant="info" @click="show_model(7)">
                            التالي
                          </b-button>
                        </b-col>
                      </b-row>
                    </b-form>
                  </validation-observer>
                </div>
              </b-overlay>
              <!-- المساحات المنزوعة   -->
              <b-overlay
                v-if="show_model_inputs == 7"
                variant="white"
                spinner-variant="primary"
                blur="0"
                opacity=".75"
                rounded="sm"
              >
                <div class="add_project_details_wrapper">
                  <validation-observer ref="addProjectRules">
                    <b-form v-if="this.hide == true">
                      <b-row class="bg-white pt-2 pb-2">
                        <b-col md="12" class="back_ground">
                          <p class="text-center">المساحات المنزوعة</p>
                        </b-col>

                        <b-col md="1"></b-col>

                        <b-col class="d-flex justify-content-center" md="8">
                        </b-col>
                        <b-col md="6">
                          <b-form-group
                            class="text-right"
                            label="المساحة المنزوعة المبنية"
                          >
                            <validation-provider
                              #default="{ errors }"
                              name="المساحة المنزوعة المبنية"
                              rules="required"
                            >
                              <b-form-input
                                v-model="form.submission.build_area"
                                :state="errors.length > 0 ? false : null"
                                placeholder="المساحة المنزوعة المبنية"
                                type="number"
                              />
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                        <b-col md="6">
                          <b-form-group
                            class="text-right"
                            label="المساحات المنزوعة غير المبنية"
                          >
                            <validation-provider
                              #default="{ errors }"
                              name="المساحات المنزوعة غير المبنية"
                              rules="required"
                            >
                              <b-form-input
                                v-model="form.submission.unbuild_area"
                                :state="errors.length > 0 ? false : null"
                                placeholder="المساحات المنزوعة غير المبنية"
                                type="number"
                              />
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                      </b-row>
                      <b-row>
                        <b-col cols="2 text-right pt-2">
                          <b-button variant="primary" @click="show_model(6)">
                            السابق
                          </b-button>
                        </b-col>

                        <b-col cols="8 text-right pt-2"> </b-col>
                        <b-col cols="2 text-right pt-2">
                          <b-button variant="info" @click="show_model(8)">
                            التالي
                          </b-button>
                          <!-- <b-button variant="danger" @click="checkSubmit()">
                              حفظ
                            </b-button> -->
                        </b-col>
                      </b-row>
                    </b-form>
                  </validation-observer>
                </div>
              </b-overlay>
              <!--  المشتمالات -->
              <b-overlay
                v-if="show_model_inputs == 8"
                variant="white"
                spinner-variant="primary"
                blur="0"
                opacity=".75"
                rounded="sm"
              >
                <div class="add_project_details_wrapper">
                  <validation-observer ref="addProjectRules">
                    <b-form v-if="this.hide == true">
                      <b-row class="bg-white pt-2 pb-2">
                        <b-col md="12" class="back_ground">
                          <p class="text-center">المشتمالات</p>
                          <!-- {{  $store.getters['dashboard/getLookups'].includes_type  }} -->
                        </b-col>
                        <b-col md="1"></b-col>
                        <b-col class="d-flex justify-content-center" md="8">
                        </b-col>
                      </b-row>
                      <b-form-group
                        class="text-right"
                        v-if="includesFormLength() == 0"
                      >
                        <b-button @click="addIncludes"> اضف</b-button>
                      </b-form-group>
                      <div
                        v-for="(item, index) in form.includesForm"
                        :key="index"
                      >
                        <b-row>
                          <b-col md="3">
                            <b-form-group
                              class="text-right"
                              label=" نوع المشتمل "
                            >
                              <validation-provider
                                #default="{ errors }"
                                name=" نوع المشتمل"
                                rules="required"
                              >
                                <v-select
                                  placeholder="نوع المشتمل"
                                  :options="
                                    $store.getters['dashboard/getLookups']
                                      .includes_type
                                  "
                                  label="name"
                                  :dir="
                                    $store.state.appConfig.layout.isRTL
                                      ? 'rtl'
                                      : 'ltr'
                                  "
                                  v-model="form.includesForm[index].build_id"
                                  :reduce="(val) => val.id"
                                >
                                </v-select>
                                <small class="text-danger" v-if="errors[0]"
                                  >هذا الحقل مطلوب</small
                                >
                              </validation-provider>
                            </b-form-group>
                          </b-col>
                          <b-col md="3">
                            <b-form-group class="text-right" label=" الوصف  ">
                              <validation-provider
                                #default="{ errors }"
                                name=" الوصف "
                                rules="required"
                              >
                                <!-- {{ $store.getters['dashboard/getLookups'].includes_type.filter((el)=>el.id == form.includesForm[index].type)[0].build_desc }} -->
                                <v-select
                                  placeholder="الوصف "
                                  :options="
                                    $store.getters[
                                      'dashboard/getLookups'
                                    ].includes_type.filter(
                                      (el) =>
                                        el.id ==
                                        form.includesForm[index].build_id
                                    )[0].build_desc
                                  "
                                  label="name"
                                  :disabled="
                                    form.includesForm[index].build_id
                                      ? false
                                      : true
                                  "
                                  :dir="
                                    $store.state.appConfig.layout.isRTL
                                      ? 'rtl'
                                      : 'ltr'
                                  "
                                  v-model="
                                    form.includesForm[index].build_desc_id
                                  "
                                  :reduce="(val) => val.id"
                                >
                                </v-select>
                                <small class="text-danger" v-if="errors[0]"
                                  >هذا الحقل مطلوب</small
                                >
                              </validation-provider>
                            </b-form-group>
                          </b-col>
                          <b-col md="2">
                            <b-form-group class="text-right" label=" الوحدة">
                              <validation-provider
                                #default="{ errors }"
                                name="الوحدة "
                                rules="required"
                              >
                                <b-form-input
                                  v-model="form.includesForm[index].qty"
                                  :state="errors.length > 0 ? false : null"
                                  placeholder=" "
                                  type="number"
                                />
                                <small class="text-danger" v-if="errors[0]"
                                  >هذا الحقل مطلوب</small
                                >
                              </validation-provider>
                            </b-form-group>
                          </b-col>
                          <b-col md="2">
                            <b-form-group class="text-right" label=" الوحدة">
                              <validation-provider
                                #default="{ errors }"
                                name="الوحدة "
                                rules="required"
                              >
                                <input
                                  type="file"
                                  name="image"
                                  @change="changeImg"
                                  accept="image/apng, image/jpeg, image/png, image/webp"
                                />
                                <small class="text-danger" v-if="errors[0]"
                                  >هذا الحقل مطلوب</small
                                >
                              </validation-provider>
                            </b-form-group>
                          </b-col>
                          <b-col md="2">
                            <b-row>
                              <b-col cols="4">
                                <b-form-group class="text-right" label=".  ">
                                  <b-button @click="addIncludes"> اضف</b-button>
                                </b-form-group>
                              </b-col>
                              <b-col cols="4">
                                <b-form-group class="text-right" label=" . ">
                                  <b-button @click="form.includesForm.pop()"
                                    >حذف</b-button
                                  >
                                </b-form-group>
                              </b-col>
                            </b-row>
                          </b-col>
                        </b-row>
                      </div>

                      <b-row>
                        <b-col cols="2 text-right pt-2">
                          <b-button variant="primary" @click="show_model(7)">
                            السابق
                          </b-button>
                        </b-col>

                        <b-col cols="8 text-right pt-2"> </b-col>
                        <b-col cols="2 text-right pt-2">
                          <!-- <b-button variant="info" @click="show_model(9)" >
                                                                التالي
                                                            </b-button> -->
                          <!-- <b-button variant="danger" @click="checkSubmit()" >
                                                                حفظ
                                                            </b-button> -->
                        </b-col>
                      </b-row>
                    </b-form>
                  </validation-observer>
                </div>
              </b-overlay>
              <!-- المرفقات   -->
              <b-overlay
                v-if="show_model_inputs == 9"
                variant="white"
                spinner-variant="primary"
                blur="0"
                opacity=".75"
                rounded="sm"
              >
                <div class="add_project_details_wrapper">
                  <validation-observer ref="addProjectRules">
                    <b-form v-if="this.hide == true">
                      <b-row class="bg-white pt-2 pb-2">
                        <b-col md="12" class="back_ground">
                          <p class="text-center">المرفقات</p>
                        </b-col>

                        <b-col md="1"></b-col>

                        <b-col class="d-flex justify-content-center" md="8">
                        </b-col>
                      </b-row>
                      <b-form-group
                        class="text-right"
                        v-if="attachLength() == 0"
                      >
                        <b-button @click="addAttachs"> اضف</b-button>
                      </b-form-group>
                      <b-row v-for="(attach, i) in form.attachs" :key="i">
                        <b-col md="4">
                          <b-form-group class="text-right" label=" المرفق ">
                            <input
                              type="file"
                              name="image"
                              accept="image/apng, image/jpeg, image/png, image/webp"
                            />
                          </b-form-group>
                          <!-- <b-form-group class="text-right" label="المرفق">
                                                            <validation-provider #default="{ errors }" name="المرفق"
                                                                rules="required">
                                                                <b-form-input v-model="attach.file"
                                                                    :state="errors.length > 0 ? false : null"
                                                                    placeholder="المرفق" type="file"/>
                                                                <small class="text-danger" v-if="errors[0]">هذا الحقل
                                                                    مطلوب</small>
                                                            </validation-provider>
                                                        </b-form-group> -->
                        </b-col>
                        <b-col md="4">
                          <b-form-group class="text-right" label="ملاحظات">
                            <validation-provider
                              #default="{ errors }"
                              name="ملاحظات"
                              rules="required"
                            >
                              <b-form-input
                                v-model="attach.note"
                                :state="errors.length > 0 ? false : null"
                                placeholder="ملاحظات"
                                type="text"
                              />
                              <small class="text-danger" v-if="errors[0]"
                                >هذا الحقل مطلوب</small
                              >
                            </validation-provider>
                          </b-form-group>
                        </b-col>
                        <b-col md="3">
                          <b-row>
                            <b-col cols="4">
                              <b-form-group class="text-right" label=".  ">
                                <b-button @click="addAttachs"> اضف</b-button>
                              </b-form-group>
                            </b-col>
                            <b-col cols="4">
                              <b-form-group class="text-right" label=" . ">
                                <b-button @click="form.attachs.pop()"
                                  >حذف</b-button
                                >
                              </b-form-group>
                            </b-col>
                          </b-row>
                        </b-col>
                      </b-row>
                      <b-row>
                        <b-col cols="2 text-right pt-2">
                          <b-button variant="primary" @click="show_model(7)">
                            السابق
                          </b-button>
                        </b-col>

                        <b-col cols="8 text-right pt-2"> </b-col>
                        <b-col cols="2 text-right pt-2">
                          <b-button variant="danger" @click="checkSubmit()">
                            حفظ
                          </b-button>
                        </b-col>
                      </b-row>
                    </b-form>
                  </validation-observer>
                </div>
              </b-overlay>
            </div>
          </b-row>
        </div>
      </b-col>
    </b-row>
    <!----------------------////////////-------------/////////////--------///////------->
  </div>
</template>

<script>
// import { mapGetters } from 'vuex'
import { ValidationProvider, ValidationObserver } from "vee-validate";
import { required, min_value } from "@validations";
import vSelect from "vue-select";
import DataTable from "@/views/components/table/DataTable";
import {
  // BOverlay,
  // https://ecb.dev.vero-cloud.com/api/
  BFormInput,
  BFormTag,
  BFormTags,
  BFormGroup,
  BForm,
  BRow,
  BCol,
  BTab,
  BTabs,
  BOverlay,
  BButton,
  BCardText,
  BCard,
  BModal,
  BFormDatepicker,
  BFormFile,
  BTable,
} from "bootstrap-vue";
import EquipmentProductivity from "@/views/dashboard/component/equipmentProductivity";
import Exports from "@/views/dashboard/component/exports";
import ManPower from "@/views/dashboard/component/manPower";
import WorkProgress from "@/views/dashboard/component/workProgress";
import lookups from "@/api/system/lookups";
import router from "@/router";
import AppInput from "./-AppInput.vue";
export default {
  props: {
    value: Array,
    fields: Array,
  },
  data() {
    return {
      form: {
        includesForm: [
          {
            build_id: null,
            build_desc_id: null,
            qty: null,
            sub_id: null,
            image: null,
          },
        ],
        owners: [
          {
            name: null,
            phone: null,
            national_id: null,
            id_type: null,
          },
        ],
        attachs: [
          {
            file: null,
            note: null,
          },
        ],

        submission: {
          restrict_border: {
            north_dir: null,
            south_dir: null,
            east_dir: null,
            west_dir: null,
            north_length: null,
            south_length: null,
            east_length: null,
            west_length: null,
          },
          contract_border_details: {
            north_dir: null,
            south_dir: null,
            east_dir: null,
            west_dir: null,
            north_length: null,
            south_length: null,
            east_length: null,
            west_length: null,
          },
          building_details: [
            {
              roof: null,
              area: null,
            },
          ],
          pro_num: null,
          pro_name: null,
          plad_num: null,
          building_number: null,
          government: null,
          area: null,
          zone: null,
          center: null,
          contract_type: null,
          contract_area: null,
          build_area: null,
          unbuild_area: null,
          total_area: null,
          planned_num: null,
          // contract_border_details: null,
          // restrict_border: null,
          contract_number: null,
          contract_date: null,
          contract_source: null,
          removed_from_building: null,
          building_type: null,
          unit: null,
          total_compensation: null,
          compensation_without_land: null,
          improvement: null,
          transportation: null,
          created_by: null,
          submission_using: null,
          submission_area: null,
          // search_text: null,
        },
      },
      show_model_inputs: 1,
      hide: true,
      // Roofs Dtat
      roofs: [
        "الدور الاول",
        "الدور الثاني",
        "الدور الثالث",
        "الدور الرابع",
        "الدور الخامس",
        "أخري",
      ],
      personality: ["زائر", "مقيم", "مواطن"],
      zones: ["A1", "A2", "A3"],
      submissionTypes: ["سكني", "تجاري", "تعليمي", "فندقي", "أرض فضاء"],
      preportyTypes: ["ايجار", "ملك"],
      submissionDesc: ["تشطيب داخلي", "تشطيب خارجي", "تشطيب داخلي و خارجي"],

      monthes: [
        { title: "يناير" },
        { title: "فبراير" },
        { title: "مارس" },
        { title: "ابريل" },
        { title: "مايو" },
        { title: "يونيو" },
        { title: "يوليو" },
        { title: "أغسطس" },
        { title: "سبتمبر" },
        { title: "اكتوبر" },
        { title: "نوفمبر" },
        { title: "ديسمبر" },
      ],
    };
  },
  components: {
    WorkProgress,
    ManPower,
    Exports,
    EquipmentProductivity,
    ValidationProvider,
    ValidationObserver,
    // BOverlay,
    BCardText,
    BCard,
    BModal,
    BFormInput,
    BFormGroup,
    BForm,
    BRow,
    BCol,
    BTab,
    BTabs,
    BOverlay,
    BTable,
    DataTable,
    BButton,
    BFormTag,
    BFormTags,
    BFormDatepicker,
    vSelect,
    BFormFile,
    AppInput,
  },
  mounted() {
    // this.includes_type  = $store.getters['dashboard/getLookups'].includes_type
    // this.$store.dispatch('dashboard/getLookups')
    //     .then((res) => {
    //         this.build_type = res.includes_type;
    //     })
  },
  methods: {
    changeImg(e) {
      console.log(e.target.files[0]);
      // this.form.includesForm.image = e.target.files[0];
    },
    // Handler Switch Between Component Function
    show_model(num) {
      this.show_model_inputs = num;
    },
    // Handler Check Inputs When Submit Form To Go To Next Step
    submitForm(refName, showModalNumber) {
      this.$refs[`${refName}`].validate().then((success) => {
        if (!success) {
          // return
          // this.show_model_inputs = showModalNumber; // will remove it
          console.log("not valid inputs");
        } else {
          console.log("yessssssssssss valid inputs");
          this.show_model_inputs = showModalNumber;
          if (refName == "contractBorderDetailsForm" && showModalNumber == 5) {
            this.submitPartOneForm();
          }
        }
      });
    },
    // Handler Add Owner Function -- Owner Details
    addOwner() {
      this.form.owners.push({ name: null, phone: null, id_type: null });
    },
    //// Start  Handler Submit Form Part One With Project Details
    submitPartOneForm() {
      console.log("Part one completed");
    },
    //// End  Handler Submit Form Part One With Project Details
    addIncludes() {
      this.form.includesForm.push({
        build_id: null,
        build_desc_id: null,
        qty: null,
      });
    },
    includesFormLength() {
      var x = this.form.includesForm;
      return x.length;
    },

    addroof() {
      this.form.submission.building_details.push({ roof: null, area: null });
    },
    ownersFormLenght() {
      var x = this.form.owners;
      return x.length;
    },
    addAttachs() {
      this.form.attachs.push({ file: null, note: null });
    },
    attachLength() {
      var x = this.form.attachs;
      return x.length;
    },
    checkSubmit() {
      console.log(this.form);

      //   this.$store;
      // .dispatch('pgc_forms/save_subs', {
      //     query: this.form,
      // })
      // .then((response) => {
      //     // console.log(response)
      //     // router.push({name:'Realtys'})
      //   this.show_model_inputs = 8 ;
      //     this.$swal({
      //         icon: 'success',
      //         title: 'تم الحفظ',
      //         showConfirmButton: false,
      //         timer: 1500,
      //     })
      //     // this.submission = response.submission;
      //         console.log(response);

      // })
      // .catch((error) => {
      //     // this.errorsdata = this.handleBackendError(error.response.data.errors)
      //     console.log(error);
      // })
    },
    // checkSubmit(){

    //     this.$swal({
    //         icon: 'info',
    //         title: 'هل انت متأكد من عملية الحفظ ',
    //         showConfirmButton: true,
    //         timer: 1500,
    //     })
    // }
  },
};
</script>

<style lang="scss" scoped>
@import "@core/scss/vue/libs/vue-select.scss";

.modal-header .close {
  display: none;
}

.main_ecb_wrapper {
  direction: rtl;

  .form_label {
    margin-bottom: 10px;
  }

  .project_details_icon_warpp {
    display: flex;
    justify-content: flex-start;
    gap: 10px;
    margin-bottom: 10px;
  }

  .project_progress_wrapper {
    border-top: 1px solid #ccc;

    .for_percentage_wrapper {
      display: flex;
      // background: bisque;
      gap: 10px;
      align-items: center;

      .form-control {
        width: 150px;
        // background: #eee;
      }
    }

    // .work_situations {
    //   .for_percentage_wrapper {
    //     display: flex;
    //     background: bisque;
    //     gap: 10px;
    //     align-items: center;
    //     .form-control {
    //       width: 150px;
    //       background: #eee;
    //     }
    //   }
    // }

    // // Start main_wrapper_for_duplicate
    .main_wrapper_for_duplicate {
      // background: #eee;
      margin-top: 15px;
      display: flex;
      align-items: center;
      justify-content: space-between;

      .part_one {
        align-items: center;
        display: flex;
        gap: 10px;

        .form-group {
          margin-bottom: 0;
        }
      }
    }
  }

  .custom-file-upload {
    cursor: pointer;
    padding: 10px;
    box-shadow: 8px -1px 3px -3px #b569bb;
    border-radius: 4px 17px 17px 4px;
  }

  .main_title_icon_wrapper {
    display: flex;
    align-items: center;
    gap: 30px;
  }

  // Part Three
  .project_part_three_wrapper {
    border-top: 1px solid #ccc;

    .part_three_heading {
      // background: #eee;
      display: flex;
      align-items: center;
      gap: 15px;
    }

    .main_container_for_values_monthes {
      padding-top: 2.2rem;

      &.main_for_workers {
        .span_work {
          min-width: 100px;
          text-align: right;
        }
      }

      .input_with_text_and_select {
        display: flex;
        align-items: center;
        gap: 15px;

        &.with_draw {
          gap: 50px;
        }

        .part_a {
          align-items: center;
          display: flex;
          gap: 8px;

          .form-group {
            margin-bottom: 0;
          }
        }

        .every_plan_month {
          align-items: center;
          display: flex;
          gap: 8px;

          .form-group {
            margin-bottom: 0;
          }
        }
      }

      .input_with_quantity {
        .part_a {
          align-items: center;
          display: flex;
          gap: 15px;
          padding-top: 1.5rem;

          .form-group {
            margin-bottom: 0;
          }

          .qunt {
            min-width: 100px;
            display: block;
          }
        }
      }
    }

    .part_two_form_three {
      .every_part {
        align-items: center;
        display: flex;
        gap: 15px;
        padding-top: 1.5rem;

        .form-group {
          margin-bottom: 0;
        }
      }
    }

    .sections_and_sectors {
      .sections {
        .part_e {
          text-align: right;
          align-items: center;
          display: flex;
          gap: 15px;
        }

        .part_d {
          text-align: right;
          align-items: center;
          display: flex;
          gap: 15px;
        }

        .minumm {
          min-width: 150px;
        }
      }
    }

    .approval_and_rejection {
      .part_d {
        text-align: right;
        align-items: center;
        display: flex;
        gap: 15px;

        .form-group {
          margin-bottom: 0;
        }

        .minumm {
          min-width: 100px;
        }
      }
    }

    .drawing_name_container {
      .part_e {
        text-align: right;
        align-items: center;
        display: flex;
        gap: 15px;

        .minumm {
          min-width: 100px;
        }
      }

      .parts_drwing_container {
        display: flex;
        justify-content: space-between;

        .every_part {
          align-items: center;
          display: flex;
          gap: 15px;
          padding-top: 1.5rem;

          .form-group {
            margin-bottom: 0;
          }

          &.problem {
            min-width: 50%;

            .form-group {
              margin-bottom: 0;
              width: 100%;
            }
          }
        }
      }
    }
  }

  .plus_icon {
    border: 1px solid #ccc;
    border-radius: 50%;
    cursor: pointer;
    width: 20px;
    height: 20px;
    padding: 2px;
  }

  //  Start Matrial
  .material_wrapper {
    border-top: 1px solid #ccc;

    .box_wrapper {
      display: flex;
      align-items: center;

      gap: 15px;
    }
  }

  // mony_wrapper
  .mony_wrapper {
    .input_with_text_and_select {
      display: flex;
      align-items: center;
      gap: 50px;

      .money_planned_text {
        min-width: 150px;
        text-align: right;
      }
    }
  }
  .back_ground {
    color: antiquewhite !important;
    padding-top: 8px;
    font-size: 20px;
    border-radius: 5px;
    background-color: #535ae7;
    margin-bottom: 10px;
  }
  .choose_images {
    .choosing_photos_ {
      display: flex;
      justify-content: center;

      .photos_label {
        border: 1px solid #ccc;
        min-width: 200px;
        text-align: center;
        height: 50px;
        line-height: 50px;
        cursor: pointer;
      }
    }
  }
}
</style>
