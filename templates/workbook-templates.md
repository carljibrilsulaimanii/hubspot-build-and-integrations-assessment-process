<!--
  workbook-templates.md -- column headers for every tab of the assessment workbook
  Author:  Jibril Sulaiman
  Date:    2026-10-19
  What:    One CSV header per tab, in the order the steps fill them. Paste each into a
           sheet tab of the same name.
  Why:     The workbook is the assessment's single output; consistent columns let the
           risk register and the scorecard reference rows by id.
-->

# Workbook templates

Create one tab per block below and paste the header line as row 1.

## Access (Step 1)

```csv
system,role_granted,granted_by,granted_on,notes
```

## Answers (Step 2)

```csv
person,group,question_number,verbatim_answer,theme,step_that_tests_it,hypothesis_id
```

## CTO questions (Step 2)

```csv
number,question,one_line_answer,proved_in_step,evidence_link
```

## Users and seats (Step 3a, 3b)

```csv
user,email,seat_type,permission_set,super_admin,team,employer,last_login,start_date,departure_plan,status,owner
```

```csv
seat_type,bought,assigned,cost_per_seat,renewal_date,approver,notes
```

## Domains (Step 3c)

```csv
host,platform,role,dns_host,ssl,redirects,primary_domain,tracking_code,tag_manager,cookie_banner,who_can_add,status,owner
```

## Email, SMS, phone (Step 3d to 3f)

```csv
item,type,setting_or_value,where_recorded,known_issue,status,owner
```

```csv
line,number_or_name,line_type,registration,inbox,agent,hubspot_channel,opt_out_handling,quiet_hours,consent_capture,rate_per_segment,workflows_using_it,status,owner
```

## Payments (Step 3g)

```csv
payment_type,processor,product,live_or_test,where_it_lands_in_hubspot,exception_handler,known_issue,status,owner
```

```csv
payment_link,product,price,after_payment_redirect,live_or_test,order_bump,pages_using_it,status,owner
```

## Objects (Step 3h)

```csv
object,type_id,standard_or_custom,record_count,count_read_at,created,creator,writers,readers,unique_key,associations,property_groups,record_customization,naming_convention,status,owner
```

## Pipelines (Step 3i)

```csv
pipeline,stage,order,win_probability,set_by_workflow,outcome_fields,owner_assignment,deal_count,amount,count_read_at,notes
```

## Workflows (Step 3j)

```csv
workflow_id,name,family,object,on_off,trigger,re_enrollment,creator,last_editor,last_edit,custom_code_actions,secrets_used,webhook_actions,third_party_actions,enrolled,errors_30d,revisions,flag,status,owner
```

## Forms and pages (Step 3k)

```csv
item,type,url_or_id,fields_or_modules,hidden_utm_fields,consent_text,triggers_workflows,head_html_blocks,payment_links,pixels,known_issue,status,owner
```

## Lists, reports, dashboards (Step 3l)

```csv
item,type,id,size_or_widgets,used_by,used_as_trigger_or_audience,quoted_where,last_viewed,status,owner
```

## Tech stack (Step 3m)

```csv
tool,role,account_owner,users,monthly_cost,hubspot_connection,status,notes
```

## Integrations, keys, secrets (Step 3n, 3o)

```csv
integration,direction,mechanism,owner,key_or_secret,test_result,failure_signal,who_gets_paged,status
```

```csv
service_key,prefix,key_id,scopes,consumers_from_view_logs,shared,deprecation_date,risk,owner
```

```csv
secret,created,purpose,consumer,confidence_observed_or_inferred,notes
```

## Outside automation (Step 3p)

```csv
automation,runs_where,trigger,writes_into_hubspot,owner,last_successful_run,status
```

## Property register (Step 4c)

```csv
object,internal_name,label,type,group,created_by,created_on,fill_rate,used_in_workflows,used_in_lists,used_in_reports,used_in_forms,used_in_cards,writer,lag,verdict,referenced_by_assets,proposed_action
```

## Trusted numbers (Step 5)

```csv
metric,definition,tool,exact_query,exclusions,current_value,read_at,owner,second_version_exists,reason_for_gap
```

## Scenarios (Step 7)

```csv
scenario,trigger,expected_trail,where_verified,record_links,verdict,failure_seen,who_would_notice
```

## Measurement chain (Step 8)

```csv
hop,what_is_captured,what_is_lost,ratio_observed,ratio_expected,owner
```

## Risk register (Steps 9, 11)

```csv
id,risk,evidence,evidence_grade,likelihood,impact,blast_radius,cost_if_down_one_week,owner,fix_build_doc_id,status
```

## Scorecard (Step 11)

```csv
area,score_1_to_5,evidence,one_change_to_move_one_point
```

## Known items (Step 11)

```csv
id,area,item,date_found,evidence,fix_build_doc_id,status
```

## Decision register (Step 10)

```csv
date,decision,decided_by,recorded_where,still_holds,notes
```

## Message assets (Step 3)

```csv
asset,type,system,status,event_or_date_referenced,links_resolved,blank_tokens,sender,unsubscribe_or_stop,owner
```

## Archive candidates (Step 3)

```csv
item,type,last_used,dependencies_checked,evidence_nothing_depends,verdict,owner
```

## Audit log (Step 3)

```csv
timestamp_local,user,category,action,asset_name,asset_id,before,after,explained_by,flag
```

## Backlog audit (Step 3)

```csv
task,owner,system,journey,status_unbuilt_halfbuilt_blocked_overdue_stale,due_on,last_update,blocked_on,finding
```

## Customer journeys (Step 3)

```csv
journey,trigger,system,workflow_or_sequence_id,on_off,last_send,messages_and_senders,owner,dark_trigger
```

## Identity precedence (Step 5)

```csv
field,hubspot_source,phone_platform_source,processor_source,winner_when_they_disagree,normalization,notes
```

## Vendor scorecard (Step 10)

```csv
vendor,open_tickets,oldest_ticket_days,promised,delivered,slipped_dates,relationship_owner_ours,relationship_owner_theirs,next_review
```

## Consent inventory (Step 9b)

```csv
form,page_url,collects_phone,consent_language,checkbox_or_implied,program_name,frequency_stated,rates_notice,stop_help,terms_link,privacy_link,not_condition_of_purchase,consent_property,timestamp_property,gap
```

## Line registration (Step 9b)

```csv
line,number,type,registration_kind,registered_use_case,actual_use,registration_status,terms_section,program_name_matches,gap
```

## Sender suppression matrix (Step 9b)

```csv
sender_or_path,honours_stop,quiet_hours_handling,timezone_basis,frequency_cap,evidence,gap
```

## Subscription types (Step 9b)

```csv
subscription_type,active,description_shown,default_for_new_contacts,set_by_forms,set_by_workflows,set_by_imports,legal_basis_recorded,emails_sent_90d,opted_in,opted_out,never_set,gap
```

## Email send audit (Step 9b)

```csv
email_name,send_date,send_method,subscription_type,content_type_marketing_or_transactional,type_matches_content,physical_address,unsubscribe_link_works,one_click_header,gap
```

## Consent audit sample (Step 9b)

```csv
contact_id,channel,message_date,sender,line,consent_record_found,consent_form,consent_date,subscription_type,within_consent,within_hours,notes
```

## Legal documents (Step 9b)

```csv
document,url,last_updated,editor,version_history_kept,owner,linked_from_forms,linked_from_checkouts,linked_from_footers,linked_from_email,links_resolve,stale_copies_found,gap
```

## Privacy policy versus stack (Step 9b)

```csv
disclosure_item,required_by,policy_says,stack_does,match,gap
```

## Sensitive data register (Step 9b)

```csv
object,property_or_store,category,hipaa_relevant,marked_sensitive,who_can_read,copied_to_systems,in_exports,in_webhooks,in_ai_tools,agreement_in_place,gap
```

## HIPAA safeguards per system (Step 9b)

```csv
system,holds_or_passes_contact_data,baa_available,baa_signed,encryption_in_transit,encryption_at_rest,unique_accounts_no_shared_logins,two_factor_enforced,least_privilege_roles,admin_review_date,audit_log_access,audit_log_exports,record_views_logged,data_minimization_fields_carried,retention_and_deletion_path,backups,session_timeout,breach_notification_terms,subprocessors_and_location,same_day_offboarding,score,evidence,gap
```

## HIPAA hop-by-hop trace (Step 9b)

```csv
hop,from_system,to_system,mechanism,fields_that_travel,sensitive_field_possible,receiving_system_score,breaks_compliance,what_closes_it,owner
```

## Banking and payment data register (Step 9b)

```csv
system_or_store,data_type_card_bank_bnpl_financial,how_it_got_there,full_or_partial,who_can_read,in_pci_scope,in_recordings_or_transcripts,retention,what_closes_it,owner
```

## HIPAA administrative and physical (Step 9b)

```csv
control,category_admin_or_physical,exists,owner,last_done,evidence,gap
```

## Lead score inventory (Step 3i2)

```csv
score,type_fit_engagement_combined_predictive_selfreported_counter,source_system,editable,inputs,caps,decay,thresholds,action_per_threshold,used_in_lists_reports_routing,owner,last_validated,verdict
```

## Score band validation (Step 3i2)

```csv
score,band,contacts,handoffs_90d,call_booked,deposit,closed_won,conversion_rate,false_positives,false_negatives,lift_vs_bottom_band,verdict
```

## Pipeline stage scorecard (Step 3i3)

```csv
pipeline,stage,order,deals,amount,buyer_commitment_not_rep_activity,exit_criterion_written,exit_criterion_property,required_properties,win_probability_set,win_probability_observed,moved_by,stale_limit_days,stale_deals,verdict
```

## Pipeline hygiene (Step 3i3)

```csv
pipeline,check,count,read_at,example_deal_ids,owner,action
```
