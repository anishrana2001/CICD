cat <<EOF > .gitlab-ci.yml
stages:
  - build
  - deploy_staging
  - deploy_prod

include:
- local: 'gitlab-ci/build_stage.yaml'
- remote: 