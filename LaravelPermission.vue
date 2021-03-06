<template>
    <div>
        <users
                v-if="section === 'users'"
                :locale="locale"
                :open-detail-function="userComponent.openDetailFunction"
                :table="userComponent.table"
                :axios="axios"
                :url-prefix="userComponent.urlPrefix || `${urlPrefix}/users`"
                :user="userComponent.user"
                :roles-download-url="userComponent.rolesDownloadUrl ? userComponent.rolesDownloadUrl : ( rolesComponent.urlPrefix ? `${rolesComponent.urlPrefix}/all` : `${urlPrefix}/roles/all`)"
                :event-bus-configuration="eventBusConfiguration"
        ></users>
        <roles
                v-else-if="section === 'roles'"
                :locale="locale"
                :table="rolesComponent.table"
                :axios="axios"
                :url-prefix="rolesComponent.urlPrefix || `${this.urlPrefix}/roles`"
                :user-download-url="rolesComponent.usersDownloadUrl ? rolesComponent.usersDownloadUrl : ( userComponent.urlPrefix ? `${userComponent.urlPrefix}/all` : `${urlPrefix}/users/all`)"
                :event-bus-configuration="eventBusConfiguration"
                :show-user-management="rolesComponent.showUserManagement"
        ></roles>
        <permissions-sections
                v-else-if="section === 'permissions'"
                :component-name="section"
                :locale="locale"
                :axios="axios"
                :url-prefix="rolesComponent.urlPrefix || `${this.urlPrefix}/permissions`"
                :event-bus-configuration="eventBusConfiguration"
        ></permissions-sections>
        <permissions-sections
                v-else-if="section === 'sections'"
                :component-name="section"
                :locale="locale"
                :axios="axios"
                :url-prefix="rolesComponent.urlPrefix || `${this.urlPrefix}/sections`"
                :event-bus-configuration="eventBusConfiguration"
        ></permissions-sections>
        <acl-manager v-else-if="section === 'manager'"
                :data-transfer-url="aclManagerConfiguration.dataTransferUrl ? aclManagerConfiguration.dataTransferUrl : ( aclManagerConfiguration.urlPrefix ? `${aclManagerConfiguration.urlPrefix}/matrix-hook` : `${this.urlPrefix}/matrix-hook`)"
                :permissions-download-url="aclManagerConfiguration.permissionsDownloadUrl ? aclManagerConfiguration.permissionsDownloadUrl : ( aclManagerConfiguration.urlPrefix ? `${aclManagerConfiguration.urlPrefix}/permissions/all` : `${this.urlPrefix}/permissions/all`)"
                :roles-download-url="aclManagerConfiguration.rolesDownloadUrl ? aclManagerConfiguration.rolesDownloadUrl : ( aclManagerConfiguration.urlPrefix ? `${aclManagerConfiguration.urlPrefix}/roles/all` : `${this.urlPrefix}/roles/all`)"
                :sections-download-url="aclManagerConfiguration.sectionsDownloadUrl ? aclManagerConfiguration.sectionsDownloadUrl : ( aclManagerConfiguration.urlPrefix ? `${aclManagerConfiguration.urlPrefix}/sections/all` : `${this.urlPrefix}/sections/all`)"
                :users-download-url="aclManagerConfiguration.usersDownloadUrl ? aclManagerConfiguration.usersDownloadUrl : ( aclManagerConfiguration.urlPrefix ? `${aclManagerConfiguration.urlPrefix}/users/all` : `${this.urlPrefix}/users/all`)"
        ></acl-manager>
        <div v-else>
            Choose a proper section prop values are: users, roles, permissions, sections, manager
        </div>
    </div>
</template>

<script lang="ts">
    import Vue, { ComponentOptions } from 'vue';

    import Users, {UserDefinition, UserField} from './components/Users.vue'

    import {AxiosStatic} from 'axios';

    import Roles, {} from './components/Roles.vue'

    import PermissionsSections, {} from './components/PermissionsSections.vue'

    import AclManager, {} from './components/AclManager.vue'

    import {Role} from './interfaces/role'

    import {TableDefinition} from './interfaces/general'

    import {EventBusConfiguration} from './interfaces/configurations'

    interface UserComponentConfiguration {
        openDetailFunction(item : any, $event? : any);
        rolesDownloadUrl?: string;
        table?: TableDefinition;
        urlPrefix?: string;
        user: UserDefinition;
    }

    interface RoleComponentConfiguration {
        usersDownloadUrl?: string;
        showUserManagement?: boolean
        table?: TableDefinition;
        urlPrefix?: string;
    }

    interface AclManagerComponentConfiguration {
        permissionsDownloadUrl?: string;
        rolesDownloadUrl?: string;
        sectionsDownloadUrl?: string;
        usersDownloadUrl?: string;
        dataTransferUrl?: string;
        urlPrefix?: string;
    }

    interface LaravelPermissionComponent extends Vue{
        aclManagerConfiguration: AclManagerComponentConfiguration;
        axios: AxiosStatic;
        eventBusConfiguration: EventBusConfiguration;
        rolesComponent: RoleComponentConfiguration;
        section: string;
        urlPrefix: string;
        userComponent: UserComponentConfiguration;
        locale: string;
    }

    export {
        AclManagerComponentConfiguration,
        EventBusConfiguration,
        Role,
        RoleComponentConfiguration,
        UserComponentConfiguration,
        UserDefinition,
        UserField,
        TableDefinition
    }

    export default {
        components: {
            'permissions-sections': PermissionsSections,
            'roles': Roles,
            'users': Users,
            'acl-manager': AclManager
        },
        props: {
            aclManagerConfiguration: {
                type: Object,
                default(){
                    return {};
                }
            },
            axios:{
                type: Object
            },
            eventBusConfiguration: {
                type: Object,
                default(){
                    return {};
                }
            },
            locale: {
                type: String,
                default: 'it'
            },
            rolesComponent: {
                type: Object,
                default(){
                    return {}
                }
            },
            section: {
                type: String,
                required: true
            },
            urlPrefix: {
                type: String
            },
            userComponent: {
                type: Object,
                required: true
            }
        }
    } as ComponentOptions<LaravelPermissionComponent>;
</script>

