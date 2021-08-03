<template>
    <div class="subscribe banner">
        <div class="page-container">
            <section class="page-content">
                <h2 v-if="jobs === true">Subscribe to career opportunities</h2>
                <h2 v-else>Subscribe to our newsletter</h2>
                <div class="subscription-container">
                    <div class="container-col" v-if="jobs === true">
                        <p>We&apos;ll let you know whenever we are hiring.  Subscribe and be the first to get your application in early!</p>
                    </div>
                    <div class="container-col" v-else>
                        <p>We&apos;ll occasionally send out a newsletter with what we&apos;re up to, blog posts, and career opportunities.</p>
                        <p>We promise not to send you too many emails. We hate spam too.</p>
                    </div>
                    <div class="container-col">
                        <form accept-charset="utf-8">
                            <div v-if="subscribeForm.errors === true" class="alert slim error">
                                <p style="margin:0">We found a few things with your form that need to be fixed.  There is specific information below about what went wrong.  If you are having trouble please let us know.  Our email address is <a href="mailto:marketing@bellese.io">marketing@bellese.io</a>.</p>
                            </div>
                            <div v-if="subscribeForm.failed === true" class="alert slim error">
                                <p style="margin:0">Uh oh!  Something went wrong.  Send us an email instead while we can subscribe you manually.  Our email address is <a href="mailto:marketing@bellese.io">marketing@bellese.io</a>.</p>
                            </div>
                            <div v-if="subscribeForm.success === true" class="alert slim success">
                                <p style="margin:0">You were successfully subscribed to {{ (jobs === true)? 'career alerts':'our newsletter' }}.</p>
                            </div>
                            <div class="row">
                                <div class="col">
                                    <span class="input-help error" v-if="subscribeForm.firstName.validation.length > 0">{{ subscribeForm.firstName.validation }}</span>
                                    <input type="text" id="subscribeFirstName" v-on:change="clearValidation(subscribeForm.firstName)"
                                           :class="(subscribeForm.firstName.value.length > 0) ? subscribeForm.firstName.additionalClasses + ' notempty' : subscribeForm.firstName.additionalClasses"
                                           v-model="subscribeForm.firstName.value"
                                           :required="subscribeForm.firstName.required"/>
                                    <label for="subscribeFirstName">First Name</label>
                                </div>
                                <div class="col">
                                    <span class="input-help error" v-if="subscribeForm.emailAddress.validation.length > 0">{{ subscribeForm.emailAddress.validation }}</span>
                                    <input type="text" id="subscribeEmailAddress" v-on:change="clearValidation(subscribeForm.emailAddress)"
                                           :class="(subscribeForm.emailAddress.value.length > 0) ? subscribeForm.emailAddress.additionalClasses + ' notempty' : subscribeForm.emailAddress.additionalClasses"
                                           v-model="subscribeForm.emailAddress.value"
                                           :required="subscribeForm.emailAddress.required"/>
                                    <label for="subscribeEmailAddress">Email Address</label>
                                </div>
                            </div>
                            <transition name="fade">
                                <div class="row" v-if="subscribeForm.subscription.jobs === true && subscribeForm.emailAddress.value.length > 0">
                                    <div class="col">
                                        <div :class="(subscriptionJobsOpen === true && subscriptionJobsActive === true)? 'multiselect active' : 'multiselect'" tabindex="0"
                                             ref="multiselect-input"
                                             v-on:focus="showSubscriptionJobMultiselect"
                                             v-click-outside="hideSubscriptionJobMultiselect">
                                            <select id="subscribeJobCategories" ref="multiselect-element" class="sr-only" multiple>
                                                <option v-for="(value, index) in subscribeForm.subscription.jobCategories" :selected="subscribeForm.subscription.jobCategories[index]">{{ index | capitalizeFirst }}</option>
                                            </select>
                                            <label for="subscribeJobCategories">What type of career are you interested in?</label>
                                            <span class="input-help" v-if="subscriptionJobsOpen !== true && selectedCategoriesArray.length > 0">Selected:
                                                <span v-for="(value, index) in selectedCategoriesArray">{{ (index === 0)? '': ', ' }}{{ value | capitalizeFirst }}</span>
                                            </span>
                                            <span class="input-help" v-else-if="subscriptionJobsOpen !== true">
                                                <span class="input-help selection">Choose Categories...</span> You will be notified of all career opportunities if you leave this blank.
                                            </span>
                                            <transition name="fade">
                                                <div class="multiselect-options"
                                                     aria-hidden="true"
                                                     v-if="subscriptionJobsOpen === true">
                                                    <span class="input-help">Choose as many as you like</span>
                                                    <ul>
                                                        <li v-for="(value, index) in subscribeForm.subscription.jobCategories">
                                                            <label class="checkbox small" :for="'subscription-category-' + index">
                                                                <input type="checkbox" :id="'subscription-category-' + index" v-model="subscribeForm.subscription.jobCategories[index]">
                                                                <span class="checkmark"></span> {{ index | capitalizeFirst }}
                                                            </label>
                                                        </li>
                                                    </ul>
                                                </div>
                                            </transition>
                                        </div>
                                    </div>
                                </div>
                            </transition>
                            <div class="row narrow">
                                <div class="col">
                                    <label :class="subscribeForm.terms.additionalClasses + ' checkbox small required'" for="subscribeAgreeToTerms">
                                        <input type="checkbox" id="subscribeAgreeToTerms"
                                               v-model="subscribeForm.terms.value"
                                               v-on:change="clearValidation(subscribeForm.terms)"
                                               v-on:focus="hideSubscriptionJobMultiselect"
                                               :required="subscribeForm.terms.required">
                                        <span class="checkmark"></span> I have reviewed the <router-link to="/privacy/">Privacy Policy</router-link> and agree to the <router-link to="/terms/">Terms of Service</router-link>.
                                    </label>
                                    <span class="input-help error" v-if="subscribeForm.terms.validation.length > 0">{{ subscribeForm.terms.validation }}</span>
                                </div>
                            </div>
                            <div class="row narrow">
                                <div class="col">
                                    <p class="fine">
                                        This site is protected using reCAPTCHA by Google.  Google&apos;s <a href="https://policies.google.com/privacy" target="_blank" rel="noreferrer">Privacy Policy</a> and <a href="https://policies.google.com/terms" target="_blank" rel="noreferrer">Terms of Service</a> apply.
                                    </p>
                                    <button :class="(subscribeForm.working)? 'button secondary icon-left':'button secondary'" type="submit" v-on:click.prevent="validateContactForm()" :disabled="subscribeForm.working"><img svg-inline svg-sprite v-if="subscribeForm.working" src="../../images/icons/spinner-third-solid.svg" aria-hidden="true" class="spinner">Subscribe</button>
                                </div>
                            </div>
                        </form>
                    </div>
                </div>
            </section>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import validator from "validator";

    export default {
        name: "Subscribe",
        data() {
            return {
                subscriptionJobsOpen: false,
                subscriptionJobsActive: false,
                selectedCategoriesArray: [],
                subscribeForm: {
                    errors: false,
                    success: false,
                    failed: false,
                    working: false,
                    firstName: {
                        value: "",
                        required: false,
                        validation: "",
                        additionalClasses: ""
                    },
                    emailAddress: {
                        value: "",
                        required: true,
                        validation: "",
                        additionalClasses: ""
                    },
                    terms: {
                        value: false,
                        required: true,
                        validation: "",
                        additionalClasses: ""
                    },
                    subscription: {
                        jobs: (this.jobs === true),
                        jobCategories: {
                            product: false,
                            engineering: false,
                            design: false,
                            operations: false
                        }
                    }
                },
            }
        },
        props: ['jobs'],
        watch: {
            subscribeForm: {
                handler: function(newVal) {
                    this.setSelectedJobCategories(newVal);
                },
                deep: true
            }
        },
        methods: {
            showSubscriptionJobMultiselect() {
                this.subscriptionJobsOpen = true;
                this.subscriptionJobsActive = true;
            },
            hideSubscriptionJobMultiselect() {
                if(this.selectedCategoriesArray.length === 0) {
                    this.subscriptionJobsOpen = false;
                }
                this.subscriptionJobsActive = false;
            },
            setSelectedJobCategories(formData) {
                let categories = formData.subscription.jobCategories;
                let result = [];
                for(const category in categories) {
                    if(categories[category] === true) {
                        result.push(category);
                    }
                }
                this.selectedCategoriesArray = result;
            },
            clearValidation(field) {
                field.validation = "";
                field.additionalClasses = "";
                this.subscribeForm.errors = false;
                this.subscribeForm.success = false;
                this.subscribeForm.failed = false;
            },
            validateContactForm() {
                // Format field check
                if(!validator.isEmail(this.subscribeForm.emailAddress.value) && this.subscribeForm.emailAddress.value.length > 0) {
                    this.subscribeForm.emailAddress.validation = "Enter a valid email address";
                    this.subscribeForm.emailAddress.additionalClasses = "error";
                    this.subscribeForm.errors = true;
                }
                if(!validator.isAlpha(this.subscribeForm.firstName.value) && this.subscribeForm.firstName.value.length > 0) {
                    this.subscribeForm.firstName.validation = "Invalid characters. Only A-z allowed.";
                    this.subscribeForm.firstName.additionalClasses = "error";
                    this.subscribeForm.errors = true;
                }
                // Required field check
                // Replaced format errors if the field is empty
                for(const field in this.subscribeForm) {
                    if(this.subscribeForm[field].required === true && this.subscribeForm[field].value.length === 0) {
                        // Add required field validation if empty
                        this.subscribeForm[field].validation = "This field is required";
                        this.subscribeForm[field].additionalClasses = "error";
                        this.subscribeForm.errors = true;
                    }
                }
                // Specifically check terms of service agreement
                if(this.subscribeForm.terms.value === false) {
                    this.subscribeForm.terms.validation = "You must agree to the Terms of Service to proceed";
                    this.subscribeForm.terms.additionalClasses = "error";
                    this.subscribeForm.errors = true;
                }

                if(this.subscribeForm.errors === true) {
                    this.subscribeForm.success = false;
                    this.subscribeForm.failed = false;
                } else {
                    this.sendSubscribe();
                }
            },
            clearContactForm() {
                this.subscribeForm.firstName.value = "";
                this.subscribeForm.emailAddress.value = "";
                this.subscribeForm.terms.value = false;
            },
            sendSubscribe: function() {
                let self = this;
                let parseCategories = "";
                let allFalse = true;
                this.subscribeForm.working = true;
                this.subscribeForm.success = false;
                this.subscribeForm.failed = false;
                for(const category in this.subscribeForm.subscription.jobCategories) {
                    if(this.subscribeForm.subscription.jobCategories[category] === true) {
                        allFalse = false;
                    }
                }
                // No Category preference, subscribe to all
                if(allFalse === true) {
                    for(const category in this.subscribeForm.subscription.jobCategories) {
                        parseCategories+= category + ' ';
                    }
                } else {
                    for(const category in this.subscribeForm.subscription.jobCategories) {
                        if(this.subscribeForm.subscription.jobCategories[category] === true) {
                            parseCategories+= category + ' ';
                        }
                    }
                }
                grecaptcha.ready(function() {
                    grecaptcha.execute(process.env.RECAPTCHA_PUBLIC_KEY, {action: 'submit'}).then(function(token) {
                        axios.post(
                            process.env.CMSURL + '/subscribe',
                            {
                                email: self.subscribeForm.emailAddress.value,
                                firstName: self.subscribeForm.firstName.value,
                                newsletter: true,
                                jobInterest: (self.jobs === true)? true: self.subscribeForm.subscription.jobs,
                                jobCategory: (self.jobs === true || self.subscribeForm.subscription.jobs)? parseCategories : '',
                                recaptcha: token
                            },
                            {
                                headers: {
                                    'content-type': 'text/json'
                                }
                            }).then(function(){
                                self.subscribeForm.success = true;
                                self.subscribeForm.working = false;
                                self.subscribeForm.failed = false;
                                self.clearContactForm();
                            }).catch(function(){
                                self.subscribeForm.failed = true;
                                self.subscribeForm.success = false;
                                self.subscribeForm.working = false;
                            });
                    });
                });
            }
        }
    }
</script>

<style scoped>

</style>