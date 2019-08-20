#!/usr/bin/env groovy
//Leave the above line alone.  It identifies this as a groovy script.
@Library('vs-build-tools') _

List<String> lvVersions = ['2017', '2018', '2019']

List<String> dependencies = ['niveristand-slsc-switch-message-library']

ni.vsbuild.PipelineExecutor.execute(this, 'veristand', lvVersions, dependencies)
diffPipeline(lvVersions[0])
