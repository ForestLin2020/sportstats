<template>
 <h5>Basketball</h5>
  <div class="career-and-season" v-if="totalsByYear.length != 0">
    <h4>Career & Season Highs</h4>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up" style="text-align: left">
        <thead>
          <tr>
            <th></th>
            <th nowrap>{{ gameYears[0] }} Season Highs</th>
            <th nowrap>Career Highs</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td nowrap>Points</td>
            <td nowrap v-if="lastSeasonHighs.pointsGame.stats.tp === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ lastSeasonHighs.pointsGame.stats.tp }}</strong>
                <span v-if="getHighestValueHappenTimes(lastSeasonHighs.pointsGame.stats.tp, 'tp') > 1"> occurred {{ getHighestValueHappenTimes(lastSeasonHighs.pointsGame.stats.tp, 'tp')}} times, last was </span>
                <span v-if="lastSeasonHighs.pointsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="lastSeasonHighs.pointsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ lastSeasonHighs.pointsGame.title }} ({{ formDateStrWithYear(lastSeasonHighs.pointsGame.event_date)}})
              </span>
            </td>
            <td nowrap v-if="carrerHighs.pointsGame.stats.tp === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ carrerHighs.pointsGame.stats.tp }}</strong>
                <span v-if="getHighestValueHappenTimes(carrerHighs.pointsGame.stats.tp, 'tp') > 1"> occurred {{ getHighestValueHappenTimes(carrerHighs.pointsGame.stats.tp, 'tp')}} times, last was </span>
                <span v-if="carrerHighs.pointsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="carrerHighs.pointsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ carrerHighs.pointsGame.title }} ({{ formDateStrWithYear(carrerHighs.pointsGame.event_date)}})
              </span>
            </td>
          </tr>
          <tr>
            <td nowrap>Field Goals Made</td>
            <td nowrap v-if="lastSeasonHighs.fieldGoalsMadeGame.stats.fgm === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ lastSeasonHighs.fieldGoalsMadeGame.stats.fgm }}</strong>
                <span v-if="getHighestValueHappenTimes(lastSeasonHighs.fieldGoalsMadeGame.stats.fgm, 'fgm') > 1"> occurred {{ getHighestValueHappenTimes(lastSeasonHighs.fieldGoalsMadeGame.stats.fgm, 'fgm')}} times, last was </span>
                <span v-if="lastSeasonHighs.fieldGoalsMadeGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="lastSeasonHighs.fieldGoalsMadeGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ lastSeasonHighs.fieldGoalsMadeGame.title }} ({{ formDateStrWithYear(lastSeasonHighs.fieldGoalsMadeGame.event_date)}})
              </span>
            </td>
            <td nowrap v-if="carrerHighs.fieldGoalsMadeGame.stats.fgm === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ carrerHighs.fieldGoalsMadeGame.stats.fgm }}</strong>
                <span v-if="getHighestValueHappenTimes(carrerHighs.fieldGoalsMadeGame.stats.fgm, 'fgm') > 1"> occurred {{ getHighestValueHappenTimes(carrerHighs.fieldGoalsMadeGame.stats.fgm, 'fgm')}} times, last was </span>
                <span v-if="carrerHighs.fieldGoalsMadeGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="carrerHighs.fieldGoalsMadeGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ carrerHighs.fieldGoalsMadeGame.title }} ({{ formDateStrWithYear(carrerHighs.fieldGoalsMadeGame.event_date)}})
              </span>
            </td>
          </tr>
          <tr>
            <td nowrap>Field Goals Attempts</td>
            <td nowrap v-if="lastSeasonHighs.fieldGoalsAttemptsGame.stats.fga === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ lastSeasonHighs.fieldGoalsAttemptsGame.stats.fga }}</strong>
                <span v-if="getHighestValueHappenTimes(lastSeasonHighs.fieldGoalsAttemptsGame.stats.fga, 'fga') > 1"> occurred {{ getHighestValueHappenTimes(lastSeasonHighs.fieldGoalsAttemptsGame.stats.fga, 'fga')}} times, last was </span>
                <span v-if="lastSeasonHighs.fieldGoalsAttemptsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="lastSeasonHighs.fieldGoalsAttemptsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ lastSeasonHighs.fieldGoalsAttemptsGame.title }} ({{ formDateStrWithYear(lastSeasonHighs.fieldGoalsAttemptsGame.event_date)}})
              </span>
            </td>
            <td nowrap v-if="carrerHighs.fieldGoalsAttemptsGame.stats.fga === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ carrerHighs.fieldGoalsAttemptsGame.stats.fga }}</strong>
                <span v-if="getHighestValueHappenTimes(carrerHighs.fieldGoalsAttemptsGame.stats.fga, 'fga') > 1"> occurred {{ getHighestValueHappenTimes(carrerHighs.fieldGoalsAttemptsGame.stats.fga, 'fga')}} times, last was </span>
                <span v-if="carrerHighs.fieldGoalsAttemptsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="carrerHighs.fieldGoalsAttemptsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ carrerHighs.fieldGoalsAttemptsGame.title }} ({{ formDateStrWithYear(carrerHighs.fieldGoalsAttemptsGame.event_date)}})
              </span>
            </td>
          </tr>
          <tr>
            <td nowrap>Three Point Field Goals Made</td>
            <td nowrap v-if="lastSeasonHighs.threePointFieldGoalsMadeGame.stats.fgm3 === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ lastSeasonHighs.threePointFieldGoalsMadeGame.stats.fgm3 }}</strong>
                <span v-if="getHighestValueHappenTimes(lastSeasonHighs.threePointFieldGoalsMadeGame.stats.fgm3, 'fgm3') > 1"> occurred {{ getHighestValueHappenTimes(lastSeasonHighs.threePointFieldGoalsMadeGame.stats.fgm3, 'fgm3')}} times, last was </span>
                <span v-if="lastSeasonHighs.threePointFieldGoalsMadeGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="lastSeasonHighs.threePointFieldGoalsMadeGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ lastSeasonHighs.threePointFieldGoalsMadeGame.title }} ({{ formDateStrWithYear(lastSeasonHighs.threePointFieldGoalsMadeGame.event_date)}})
              </span>
            </td>
            <td nowrap v-if="carrerHighs.threePointFieldGoalsMadeGame.stats.fgm3 === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ carrerHighs.threePointFieldGoalsMadeGame.stats.fgm3 }}</strong>
                <span v-if="getHighestValueHappenTimes(carrerHighs.threePointFieldGoalsMadeGame.stats.fgm3, 'fgm3') > 1"> occurred {{ getHighestValueHappenTimes(carrerHighs.threePointFieldGoalsMadeGame.stats.fgm3, 'fgm3')}} times, last was </span>
                <span v-if="carrerHighs.threePointFieldGoalsMadeGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="carrerHighs.threePointFieldGoalsMadeGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ carrerHighs.threePointFieldGoalsMadeGame.title }} ({{ formDateStrWithYear(carrerHighs.threePointFieldGoalsMadeGame.event_date)}})
              </span>
            </td>
          </tr>
          <tr>
            <td nowrap>Three Point Field Goals Attempts</td>
            <td nowrap v-if="lastSeasonHighs.threePointFieldGoalsAttemptsGame.stats.fga3 === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ lastSeasonHighs.threePointFieldGoalsAttemptsGame.stats.fga3 }}</strong>
                <span v-if="getHighestValueHappenTimes(lastSeasonHighs.threePointFieldGoalsAttemptsGame.stats.fga3, 'fga3') > 1"> occurred {{ getHighestValueHappenTimes(lastSeasonHighs.threePointFieldGoalsAttemptsGame.stats.fga3, 'fga3')}} times, last was </span>
                <span v-if="lastSeasonHighs.threePointFieldGoalsAttemptsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="lastSeasonHighs.threePointFieldGoalsAttemptsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ lastSeasonHighs.threePointFieldGoalsAttemptsGame.title }} ({{ formDateStrWithYear(lastSeasonHighs.threePointFieldGoalsAttemptsGame.event_date)}})
              </span>
            </td>
            <td nowrap v-if="carrerHighs.threePointFieldGoalsAttemptsGame.stats.fga3 === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ carrerHighs.threePointFieldGoalsAttemptsGame.stats.fga3 }}</strong>
                <span v-if="getHighestValueHappenTimes(carrerHighs.threePointFieldGoalsAttemptsGame.stats.fga3, 'fga3') > 1"> occurred {{ getHighestValueHappenTimes(carrerHighs.threePointFieldGoalsAttemptsGame.stats.fga3, 'fga3')}} times, last was </span>
                <span v-if="carrerHighs.threePointFieldGoalsAttemptsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="carrerHighs.threePointFieldGoalsAttemptsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ carrerHighs.threePointFieldGoalsAttemptsGame.title }} ({{ formDateStrWithYear(carrerHighs.threePointFieldGoalsAttemptsGame.event_date)}})
              </span>
            </td>
          </tr>
          <tr>
            <td nowrap>Free Throws Made</td>
            <td nowrap v-if="lastSeasonHighs.freeThrowsMadeGame.stats.ftm === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ lastSeasonHighs.freeThrowsMadeGame.stats.ftm }}</strong>
                <span v-if="getHighestValueHappenTimes(lastSeasonHighs.freeThrowsMadeGame.stats.ftm, 'ftm') > 1"> occurred {{ getHighestValueHappenTimes(lastSeasonHighs.freeThrowsMadeGame.stats.ftm, 'ftm')}} times, last was </span>
                <span v-if="lastSeasonHighs.freeThrowsMadeGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="lastSeasonHighs.freeThrowsMadeGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ lastSeasonHighs.freeThrowsMadeGame.title }} ({{ formDateStrWithYear(lastSeasonHighs.freeThrowsMadeGame.event_date)}})
              </span>
            </td>
            <td nowrap v-if="carrerHighs.freeThrowsMadeGame.stats.ftm === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ carrerHighs.freeThrowsMadeGame.stats.ftm }}</strong>
                <span v-if="getHighestValueHappenTimes(carrerHighs.freeThrowsMadeGame.stats.ftm, 'ftm') > 1"> occurred {{ getHighestValueHappenTimes(carrerHighs.freeThrowsMadeGame.stats.ftm, 'ftm')}} times, last was </span>
                <span v-if="carrerHighs.freeThrowsMadeGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="carrerHighs.freeThrowsMadeGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ carrerHighs.freeThrowsMadeGame.title }} ({{ formDateStrWithYear(carrerHighs.freeThrowsMadeGame.event_date)}})
              </span>
            </td>
          </tr>
          <tr>
            <td nowrap>Free Throw Attempts</td>
            <td nowrap v-if="lastSeasonHighs.freeThrowAttemptsGame.stats.fta === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ lastSeasonHighs.freeThrowAttemptsGame.stats.fta }}</strong>
                <span v-if="getHighestValueHappenTimes(lastSeasonHighs.freeThrowAttemptsGame.stats.fta, 'fta') > 1"> occurred {{ getHighestValueHappenTimes(lastSeasonHighs.freeThrowAttemptsGame.stats.fta, 'fta')}} times, last was </span>
                <span v-if="lastSeasonHighs.freeThrowAttemptsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="lastSeasonHighs.freeThrowAttemptsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ lastSeasonHighs.freeThrowAttemptsGame.title }} ({{ formDateStrWithYear(lastSeasonHighs.freeThrowAttemptsGame.event_date)}})
              </span>
            </td>
            <td nowrap v-if="carrerHighs.freeThrowAttemptsGame.stats.fta === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ carrerHighs.freeThrowAttemptsGame.stats.fta }}</strong>
                <span v-if="getHighestValueHappenTimes(carrerHighs.freeThrowAttemptsGame.stats.fta, 'fta') > 1"> occurred {{ getHighestValueHappenTimes(carrerHighs.freeThrowAttemptsGame.stats.fta, 'fta')}} times, last was </span>
                <span v-if="carrerHighs.freeThrowAttemptsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="carrerHighs.freeThrowAttemptsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ carrerHighs.freeThrowAttemptsGame.title }} ({{ formDateStrWithYear(carrerHighs.freeThrowAttemptsGame.event_date)}})
              </span>
            </td>
          </tr>
          <tr>
            <td nowrap>Offensive Rebounds</td>
            <td nowrap v-if="lastSeasonHighs.offensiveReboundsGame.stats.oreb === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ lastSeasonHighs.offensiveReboundsGame.stats.oreb }}</strong>
                <span v-if="getHighestValueHappenTimes(lastSeasonHighs.offensiveReboundsGame.stats.oreb, 'oreb') > 1"> occurred {{ getHighestValueHappenTimes(lastSeasonHighs.offensiveReboundsGame.stats.oreb, 'oreb')}} times, last was </span>
                <span v-if="lastSeasonHighs.offensiveReboundsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="lastSeasonHighs.offensiveReboundsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ lastSeasonHighs.offensiveReboundsGame.title }} ({{ formDateStrWithYear(lastSeasonHighs.offensiveReboundsGame.event_date)}})
              </span>
            </td>
            <td nowrap v-if="carrerHighs.offensiveReboundsGame.stats.oreb === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ carrerHighs.offensiveReboundsGame.stats.oreb }}</strong>
                <span v-if="getHighestValueHappenTimes(carrerHighs.offensiveReboundsGame.stats.oreb, 'oreb') > 1"> occurred {{ getHighestValueHappenTimes(carrerHighs.offensiveReboundsGame.stats.oreb, 'oreb')}} times, last was </span>
                <span v-if="carrerHighs.offensiveReboundsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="carrerHighs.offensiveReboundsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ carrerHighs.offensiveReboundsGame.title }} ({{ formDateStrWithYear(carrerHighs.offensiveReboundsGame.event_date)}})
              </span>
            </td>
          </tr>
          <tr>
            <td nowrap>Defensive Rebounds</td>
            <td nowrap v-if="lastSeasonHighs.defensiveReboundsGame.stats.dreb === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ lastSeasonHighs.defensiveReboundsGame.stats.dreb }}</strong>
                <span v-if="getHighestValueHappenTimes(lastSeasonHighs.defensiveReboundsGame.stats.dreb, 'dreb') > 1"> occurred {{ getHighestValueHappenTimes(lastSeasonHighs.defensiveReboundsGame.stats.dreb, 'dreb')}} times, last was </span>
                <span v-if="lastSeasonHighs.defensiveReboundsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="lastSeasonHighs.defensiveReboundsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ lastSeasonHighs.defensiveReboundsGame.title }} ({{ formDateStrWithYear(lastSeasonHighs.defensiveReboundsGame.event_date)}})
              </span>
            </td>
            <td nowrap v-if="carrerHighs.defensiveReboundsGame.stats.dreb === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ carrerHighs.defensiveReboundsGame.stats.dreb }}</strong>
                <span v-if="getHighestValueHappenTimes(carrerHighs.defensiveReboundsGame.stats.dreb, 'dreb') > 1"> occurred {{ getHighestValueHappenTimes(carrerHighs.defensiveReboundsGame.stats.dreb, 'dreb')}} times, last was </span>
                <span v-if="carrerHighs.defensiveReboundsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="carrerHighs.defensiveReboundsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ carrerHighs.defensiveReboundsGame.title }} ({{ formDateStrWithYear(carrerHighs.defensiveReboundsGame.event_date)}})
              </span>
            </td>
          </tr>
          <tr>
            <td nowrap>Total Rebounds</td>
            <td nowrap v-if="lastSeasonHighs.totalReboundsGame.stats.treb === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ lastSeasonHighs.totalReboundsGame.stats.treb }}</strong>
                <span v-if="getHighestValueHappenTimes(lastSeasonHighs.totalReboundsGame.stats.treb, 'treb') > 1"> occurred {{ getHighestValueHappenTimes(lastSeasonHighs.totalReboundsGame.stats.treb, 'treb')}} times, last was </span>
                <span v-if="lastSeasonHighs.totalReboundsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="lastSeasonHighs.totalReboundsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ lastSeasonHighs.totalReboundsGame.title }} ({{ formDateStrWithYear(lastSeasonHighs.totalReboundsGame.event_date)}})
              </span>
            </td>
            <td nowrap v-if="carrerHighs.totalReboundsGame.stats.treb === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ carrerHighs.totalReboundsGame.stats.treb }}</strong>
                <span v-if="getHighestValueHappenTimes(carrerHighs.totalReboundsGame.stats.treb, 'treb') > 1"> occurred {{ getHighestValueHappenTimes(carrerHighs.totalReboundsGame.stats.treb, 'treb')}} times, last was </span>
                <span v-if="carrerHighs.totalReboundsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="carrerHighs.totalReboundsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ carrerHighs.totalReboundsGame.title }} ({{ formDateStrWithYear(carrerHighs.totalReboundsGame.event_date)}})
              </span>
            </td>
          </tr>
          <tr>
            <td nowrap>Assists</td>
            <td nowrap v-if="lastSeasonHighs.totalReboundsGame.stats.ast === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ lastSeasonHighs.assistsGame.stats.ast }}</strong>
                <span v-if="getHighestValueHappenTimes(lastSeasonHighs.assistsGame.stats.ast, 'ast') > 1"> occurred {{ getHighestValueHappenTimes(lastSeasonHighs.assistsGame.stats.ast, 'ast')}} times, last was </span>
                <span v-if="lastSeasonHighs.assistsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="lastSeasonHighs.assistsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ lastSeasonHighs.assistsGame.title }} ({{ formDateStrWithYear(lastSeasonHighs.assistsGame.event_date)}})
              </span>
            </td>
            <td nowrap v-if="carrerHighs.totalReboundsGame.stats.ast === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ carrerHighs.assistsGame.stats.ast }}</strong>
                <span v-if="getHighestValueHappenTimes(carrerHighs.assistsGame.stats.ast, 'ast') > 1"> occurred {{ getHighestValueHappenTimes(carrerHighs.assistsGame.stats.ast, 'ast')}} times, last was </span>
                <span v-if="carrerHighs.assistsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="carrerHighs.assistsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ carrerHighs.assistsGame.title }} ({{ formDateStrWithYear(carrerHighs.assistsGame.event_date)}})
              </span>
            </td>
          </tr>
          <tr>
            <td nowrap>Steals</td>
            <td nowrap v-if="lastSeasonHighs.stealsGame.stats.stl === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ lastSeasonHighs.stealsGame.stats.stl }}</strong>
                <span v-if="getHighestValueHappenTimes(lastSeasonHighs.stealsGame.stats.stl, 'stl') > 1"> occurred {{ getHighestValueHappenTimes(lastSeasonHighs.stealsGame.stats.stl, 'stl')}} times, last was </span>
                <span v-if="lastSeasonHighs.stealsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="lastSeasonHighs.stealsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ lastSeasonHighs.stealsGame.title }} ({{ formDateStrWithYear(lastSeasonHighs.stealsGame.event_date)}})
              </span>
            </td>
            <td nowrap v-if="carrerHighs.stealsGame.stats.stl === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ carrerHighs.stealsGame.stats.stl }}</strong>
                <span v-if="getHighestValueHappenTimes(carrerHighs.stealsGame.stats.stl, 'stl') > 1"> occurred {{ getHighestValueHappenTimes(carrerHighs.stealsGame.stats.stl, 'stl')}} times, last was </span>
                <span v-if="carrerHighs.stealsGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="carrerHighs.stealsGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ carrerHighs.stealsGame.title }} ({{ formDateStrWithYear(carrerHighs.stealsGame.event_date)}})
              </span>
            </td>
          </tr>
          <tr>
            <td nowrap>Blocks</td>
            <td nowrap v-if="lastSeasonHighs.blocksGame.stats.blk === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ lastSeasonHighs.blocksGame.stats.blk }}</strong>
                <span v-if="getHighestValueHappenTimes(lastSeasonHighs.blocksGame.stats.blk, 'blk') > 1"> occurred {{ getHighestValueHappenTimes(lastSeasonHighs.blocksGame.stats.blk, 'blk')}} times, last was </span>
                <span v-if="lastSeasonHighs.blocksGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="lastSeasonHighs.blocksGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ lastSeasonHighs.blocksGame.title }} ({{ formDateStrWithYear(lastSeasonHighs.blocksGame.event_date)}})
              </span>
            </td>
            <td nowrap v-if="carrerHighs.blocksGame.stats.blk === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ carrerHighs.blocksGame.stats.blk }}</strong>
                <span v-if="getHighestValueHappenTimes(carrerHighs.blocksGame.stats.blk, 'blk') > 1"> occurred {{ getHighestValueHappenTimes(carrerHighs.blocksGame.stats.blk, 'blk')}} times, last was </span>
                <span v-if="carrerHighs.blocksGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="carrerHighs.blocksGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ carrerHighs.blocksGame.title }} ({{ formDateStrWithYear(carrerHighs.blocksGame.event_date)}})
              </span>
            </td>
          </tr>
          <tr>
            <td nowrap>Minutes</td>
            <td v-if="lastSeasonHighs.minutesGame.stats.min === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ lastSeasonHighs.minutesGame.stats.min }}</strong>
                <span v-if="getHighestValueHappenTimes(lastSeasonHighs.minutesGame.stats.min, 'min') > 1"> occurred {{ getHighestValueHappenTimes(lastSeasonHighs.minutesGame.stats.min, 'min')}} times, last was </span>
                <span v-if="lastSeasonHighs.minutesGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="lastSeasonHighs.minutesGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ lastSeasonHighs.minutesGame.title }} ({{ formDateStrWithYear(lastSeasonHighs.minutesGame.event_date)}})
              </span>
            </td>
            <td v-if="carrerHighs.minutesGame.stats.min === 0">-</td>
            <td nowrap v-else>
              <span>
                <strong>{{ carrerHighs.minutesGame.stats.min }}</strong>
                <span v-if="getHighestValueHappenTimes(carrerHighs.minutesGame.stats.min, 'min') > 1"> occurred {{ getHighestValueHappenTimes(carrerHighs.minutesGame.stats.min, 'min')}} times, last was </span>
                <span v-if="carrerHighs.minutesGame.venue.neutralgame === 'Y'"> vs.</span>
                <span v-else-if="carrerHighs.minutesGame.venue.homeid === 'BYU'"> </span>
                <span v-else> @</span>
                {{ carrerHighs.minutesGame.title }} ({{ formDateStrWithYear(carrerHighs.minutesGame.event_date)}})
              </span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <h4>Career & Season Stats</h4>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
            <th nowrap>Year</th>
            <th nowrap>GP</th>
            <th nowrap>GS</th>
            <th nowrap>MIN</th>
            <th nowrap>AVG</th>
            <th nowrap>FG-FGA</th>
            <th nowrap>FG%</th>
            <th nowrap>3P-3PA</th>
            <th nowrap>3P%</th>
            <th nowrap>FT-FTA</th>
            <th nowrap>FT%</th>
            <th nowrap>OFF</th>
            <th nowrap>DEF</th>
            <th nowrap>REB</th>
            <th nowrap>AVG</th>
            <th nowrap>AST</th>
            <th nowrap>AVG</th>
            <th nowrap>BLK</th>
            <th nowrap>STL</th>
            <th nowrap>PF</th>
            <th nowrap>TO</th>
            <th nowrap>PTS</th>
            <th nowrap>AVG</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(totals,index) in totalsByYear" :key="index">
            <td nowrap>{{totals.schedule_year}}</td>
            <td nowrap>{{totals.gp }}</td>
            <td nowrap>{{totals.gs }}</td>
            <td nowrap>{{totals.stats.min }}</td>
            <td nowrap>{{totals.stats.minavg }}</td>
            <td nowrap>{{totals.stats.fgm }}-{{ totals.stats.fga  }}</td>
            <td nowrap>{{totals.stats.fgm_fga_percent }}</td>
            <td nowrap>{{totals.stats.fgm3 }}-{{totals.stats.fga3 }}</td>
            <td nowrap>{{totals.stats.fgm3_fga3_percent }}</td>
            <td nowrap>{{totals.stats.ftm }}-{{totals.stats.fta }}</td>
            <td nowrap>{{totals.stats.ftm_fta_percent}}</td>
            <td nowrap>{{totals.stats.oreb}}</td>
            <td nowrap>{{totals.stats.dreb}}</td>
            <td nowrap>{{totals.stats.treb}}</td>
            <td nowrap>{{totals.stats.trebavg  }}</td>
            <td nowrap>{{totals.stats.ast}}</td>
            <td nowrap>{{totals.stats.astavg }}</td>
            <td nowrap>{{totals.stats.blk}}</td>
            <td nowrap>{{totals.stats.stl}}</td>
            <td nowrap>{{totals.stats.pf}}</td>
            <td nowrap>{{totals.stats.to}}</td>
            <td nowrap>{{totals.stats.tp}}</td>
            <td nowrap>{{totals.stats.tpavg }}</td>
          </tr>
        </tbody>
      </table>
    </div>

    <div v-for="(year, index) in gameYears" :key="index">
      <div class="table-responsive mt-3">
        <table class=" table-striped table-sm table-condensed table table-hover table-bordered total_up">
          <thead>
            <tr>
              <th nowrap>{{ year }} Opponent</th>
              <th nowrap>Date</th>
              <th nowrap>Result</th>
              <th nowrap>MIN</th>
              <th nowrap>FG-FGA</th>
              <th nowrap>FG%</th>
              <th nowrap>3P-3PA</th>
              <th nowrap>3P%</th>
              <th nowrap>FT-FTA</th>
              <th nowrap>FT%</th>
              <th nowrap>OFF</th>
              <th nowrap>DEF</th>
              <th nowrap>REB</th>
              <th nowrap>AST</th>
              <th nowrap>BLK</th>
              <th nowrap>STL</th>
              <th nowrap>PF</th>
              <th nowrap>TO</th>
              <th nowrap>PTS</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(game, _id) in gamesFilterEventsByYear(year)" :key="_id">
              <td nowrap style="text-align: left; color:#00a2e8;">
                <span v-if="game.venue.homename != 'BYU' && game.venue.neutralgame != 'Y' ">@</span>{{game.title}}
              </td>
              <td nowrap>{{ formDateStr(game.event_date) }}</td>
              <td nowrap v-if="(game.byu_score - 0) > (game.opp_score - 0)"><b style="color: green">W </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td nowrap v-if="(game.byu_score - 0) == (game.opp_score - 0)"><b style="color: blue">T </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td nowrap v-if="(game.byu_score - 0) < (game.opp_score - 0)"><b style="color: red">L </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td nowrap>{{ game.stats.min }}</td>
              <td nowrap>{{ game.stats.fgm }}-{{ game.stats.fga }}</td>
              <td nowrap>{{ game.stats.fgm_fga_percent }}</td>
              <td nowrap>{{ game.stats.fgm3 }}-{{ game.stats.fga3 }}</td>
              <td nowrap>{{ game.stats.fgm3_fga3_percent}}</td>
              <td nowrap>{{ game.stats.ftm }}-{{ game.stats.fta }}</td>
              <td nowrap>{{ game.stats.ftm_fta_percent }}</td>
              <td nowrap>{{ game.stats.oreb }}</td>
              <td nowrap>{{ game.stats.dreb }}</td>
              <td nowrap>{{ game.stats.treb }}</td>
              <td nowrap>{{ game.stats.ast }}</td>
              <td nowrap>{{ game.stats.blk }}</td>
              <td nowrap>{{ game.stats.stl }}</td>
              <td nowrap>{{ game.stats.pf }}</td>
              <td nowrap>{{ game.stats.to }}</td>
              <td nowrap>{{ game.stats.tp }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Basketball',
  props: ['selected', 'gamesRecordPlayerIn'],
  data () {
    return {
      // For UI to format dates
      months: [
        'Jan.', // "January",
        'Feb.', // "February",
        'Mar.', // "March",
        'Apr.', // "April",
        'May.', // "May",
        'Jun.', // "June",
        'Jul.', // "July",
        'Aug.', // "August",
        'Sep.', // "September",
        'Oct.', // "October",
        'Nov.', // "November",
        'Dec.' // "December",
      ],
      totalsByYear: [],
      gamesRecordPlayerInCleared: [],
      gameYears: [],
      lastSeasonHighs: [], // highest value based on last year
      carrerHighs: [] // highest value based on whole carrer year
    }
  },
  computed: {
    // you can do some thing here
  },
  mounted () {
    // you can do some thing here
  },
  methods: {
    reorganizeGames () {
      // clear the old data
      if (this.gamesRecordPlayerInCleared) {
        this.gamesRecordPlayerInCleared = []
        this.gameYears = []
      }

      const data = this.gamesRecordPlayerIn
      for (var i = 0; i < data.length; i++) {
        // x.id === 'BYU' or x.name === 'BYU' or x.name === 'BY'
        const byuTeamIndex = data[i].bbgame.team.findIndex(x => (x.id === 'BYU' || x.name === 'BYU')) // find index in array
        const oppoTeamIndex = data[i].bbgame.team.findIndex(x => x.id !== 'BYU') // find index in array
        const byuTeamStat = data[i].bbgame.team[byuTeamIndex]
        const playerIndex = byuTeamStat.player.findIndex(x => x.player_nid === this.selected.athleteNid - 0)
        const playerStat = byuTeamStat.player[playerIndex]

        // Insert game's info into player's info -> Makes template much easier to render data
        // -> Makes data into same layer if they are stored in different place
        // -> response.data[i].bbgame.team[0].player[p].nid = response.data[i].nid
        playerStat.nid = data[i].nid
        playerStat.title = data[i].title
        playerStat.schedule_year = data[i].schedule_year
        playerStat.event_date = data[i].event_date
        playerStat.scores = data[i].bbgame.scores
        playerStat.venue = data[i].bbgame.venue
        playerStat.linescore = data[i].bbgame.team[byuTeamIndex].linescore
        playerStat.opp_score = data[i].bbgame.team[oppoTeamIndex].linescore.score
        playerStat.byu_score = data[i].bbgame.team[byuTeamIndex].linescore.score
        this.gamesRecordPlayerInCleared.push(playerStat)

        // ===== store different years for grouping stats table =====
        if (this.gameYears.indexOf(playerStat.schedule_year) === -1) {
          this.gameYears.push(playerStat.schedule_year)
        }
      }

      // ===== Order the games' year for table order =====
      this.fillEmptyCategoryandStats()
      this.gameYears.sort()
      this.gameYears.reverse()
      this.calTotalsByYear(this.gameYears)
      this.gamesRecordPlayerInCleared.sort(this.dateSort)

      console.log('selected', this.selected)
      console.log('gamesRecordPlayerInCleared', this.gamesRecordPlayerInCleared)
      console.log('gameYears', this.gameYears)
    },
    formDateStr (str) {
      var date = new Date(str)
      // need to double check if area different then show different data
      date.setHours(date.getHours() - 6)
      var dateStr = this.months[date.getMonth()] + ' ' + date.getDate()
      return dateStr
    },
    formDateStrWithYear (str) {
      var date = new Date(str)
      // need to double check if area different then show different data
      date.setHours(date.getHours() - 6)
      var dateStr = this.months[date.getMonth()] + ' ' + date.getDate() + ', ' + date.getFullYear()
      return dateStr
    },
    gamesFilterEventsByYear (year) {
      const games = this.gamesRecordPlayerInCleared.filter(game => game.schedule_year === year)
      return games
    },
    fillEmptyCategoryandStats () {
      const games = this.gamesRecordPlayerInCleared
      for (var i = 0; i < games.length; i++) {
        if (!games[i].stats) games[i].stats = {}
        if (!games[i].stats.min) games[i].stats.min = 0
        if (!games[i].stats.fgm) games[i].stats.fgm = 0
        if (!games[i].stats.fga || games[i].stats.fga === '0') {
          games[i].stats.fga = 0
          games[i].stats.fgm_fga_percent = '0.000'
        } else {
          games[i].stats.fgm_fga_percent = (games[i].stats.fgm / games[i].stats.fga).toFixed(3)
        }
        if (!games[i].stats.fgm3) games[i].stats.fgm3 = 0
        if (!games[i].stats.fga3 || games[i].stats.fta3 === '0') {
          games[i].stats.fga3 = 0
          games[i].stats.fgm3_fga3_percent = '0.000'
        } else {
          games[i].stats.fgm3_fga3_percent = (games[i].stats.fgm3 / games[i].stats.fga3).toFixed(3)
        }
        if (!games[i].stats.ftm) games[i].stats.ftm = 0
        if (!games[i].stats.fta || games[i].stats.fta === '0') {
          games[i].stats.fta = 0
          games[i].stats.ftm_fta_percent = '0.000'
        } else {
          games[i].stats.ftm_fta_percent = (games[i].stats.ftm / games[i].stats.fta).toFixed(3)
        }
        if (!games[i].stats.oreb) games[i].stats.oreb = 0
        if (!games[i].stats.dreb) games[i].stats.dreb = 0
        if (!games[i].stats.treb) games[i].stats.treb = 0
        if (!games[i].stats.ast) games[i].stats.ast = 0
        if (!games[i].stats.blk) games[i].stats.blk = 0
        if (!games[i].stats.stl) games[i].stats.stl = 0
        if (!games[i].stats.pf) games[i].stats.pf = 0
        if (!games[i].stats.to) games[i].stats.to = 0
        if (!games[i].stats.tp) games[i].stats.tp = 0
      }
      this.gamesRecordPlayerInCleared = games
    },
    dateSort (a, b) {
      if (a.event_date < b.event_date) {
        return -1
      }
      if (a.event_date > b.event_date) {
        return 1
      }
      return 0
    },
    calTotalsByYear (years) {
      this.totalsByYear = []
      for (let i = 0; i < years.length; i++) {
        const gamesByYear = this.gamesFilterEventsByYear(years[i])
        const gamesTotalByYear = {
          schedule_year: years[i],
          gp: this.getGP(gamesByYear),
          gs: this.getGS(gamesByYear),
          stats: {
            min: this.calTotal(gamesByYear, 'stats', 'min'),
            minavg: this.getAVG(this.calTotal(gamesByYear, 'stats', 'min'), this.getGP(gamesByYear)),
            fgm: this.calTotal(gamesByYear, 'stats', 'fgm'),
            fga: this.calTotal(gamesByYear, 'stats', 'fga'),
            fgm_fga_percent: this.getAVG(this.calTotal(gamesByYear, 'stats', 'fgm'), this.calTotal(gamesByYear, 'stats', 'fga')),
            fgm3: this.calTotal(gamesByYear, 'stats', 'fgm3'),
            fga3: this.calTotal(gamesByYear, 'stats', 'fga3'),
            fgm3_fga3_percent: this.getAVG(this.calTotal(gamesByYear, 'stats', 'fgm3'), this.calTotal(gamesByYear, 'stats', 'fga3')),
            ftm: this.calTotal(gamesByYear, 'stats', 'ftm'),
            fta: this.calTotal(gamesByYear, 'stats', 'fta'),
            ftm_fta_percent: this.getAVG(this.calTotal(gamesByYear, 'stats', 'ftm'), this.calTotal(gamesByYear, 'stats', 'fta')),
            oreb: this.calTotal(gamesByYear, 'stats', 'oreb'),
            dreb: this.calTotal(gamesByYear, 'stats', 'dreb'),
            treb: this.calTotal(gamesByYear, 'stats', 'treb'),
            trebavg: this.getAVG(this.calTotal(gamesByYear, 'stats', 'treb'), this.getGP(gamesByYear)),
            ast: this.calTotal(gamesByYear, 'stats', 'ast'),
            astavg: this.getAVG(this.calTotal(gamesByYear, 'stats', 'ast'), this.getGP(gamesByYear)),
            blk: this.calTotal(gamesByYear, 'stats', 'blk'),
            stl: this.calTotal(gamesByYear, 'stats', 'stl'),
            pf: this.calTotal(gamesByYear, 'stats', 'pf'),
            to: this.calTotal(gamesByYear, 'stats', 'to'),
            tp: this.calTotal(gamesByYear, 'stats', 'tp'),
            tpavg: this.getAVG(this.calTotal(gamesByYear, 'stats', 'tp'), this.getGP(gamesByYear))
          }
        }
        this.totalsByYear.push(gamesTotalByYear)
        this.getHighs()
      }
    },
    getGP (games) {
      let total = 0
      for (let i = 0; i < games.length; i++) {
        if (games[i] && games[i].gp) {
          total += games[i].gp - 0
        }
      }
      return total
    },
    getGS (games) {
      let total = 0
      for (let i = 0; i < games.length; i++) {
        if (games[i] && games[i].gs) {
          total += games[i].gs - 0
        }
      }
      return total
    },
    getAVG (numerator, denominator) {
      if (denominator === 0) return 0
      const avg = (numerator / denominator).toFixed(3)
      return avg
    },
    calTotal (games, categoryKey, statsKey) {
      let total = 0
      for (let i = 0; i < games.length; i++) {
        if (games[i] && games[i][categoryKey] && games[i][categoryKey][statsKey]) {
          total += games[i][categoryKey][statsKey] - 0
        }
      }
      return total
    },
    getTotalsByYear (year) {
      return this.totalsByYear.filter(total => total.schedule_year === year)
    },
    getHighs () {
      this.lastSeasonHighs = []
      this.carrerHighs = []

      let game = this.gamesRecordPlayerInCleared.sort(this.dateSort)
      this.carrerHighs = {
        pointsGame: this.getHighestRecord(game, 'tp'),
        fieldGoalsMadeGame: this.getHighestRecord(game, 'fgm'),
        fieldGoalsAttemptsGame: this.getHighestRecord(game, 'fga'),
        threePointFieldGoalsMadeGame: this.getHighestRecord(game, 'fgm3'),
        threePointFieldGoalsAttemptsGame: this.getHighestRecord(game, 'fga3'),
        freeThrowsMadeGame: this.getHighestRecord(game, 'ftm'),
        freeThrowAttemptsGame: this.getHighestRecord(game, 'fta'),
        offensiveReboundsGame: this.getHighestRecord(game, 'oreb'),
        defensiveReboundsGame: this.getHighestRecord(game, 'dreb'),
        totalReboundsGame: this.getHighestRecord(game, 'treb'),
        assistsGame: this.getHighestRecord(game, 'ast'),
        stealsGame: this.getHighestRecord(game, 'stl'),
        blocksGame: this.getHighestRecord(game, 'blk'),
        minutesGame: this.getHighestRecord(game, 'min')
      }
      console.log(' this.gameYears in here', this.gameYears)
      const lastYear = this.gameYears[0]
      game = this.gamesFilterEventsByYear(lastYear)
      game.sort(this.dateSort)
      this.lastSeasonHighs = {
        pointsGame: this.getHighestRecord(game, 'tp'),
        fieldGoalsMadeGame: this.getHighestRecord(game, 'fgm'),
        fieldGoalsAttemptsGame: this.getHighestRecord(game, 'fga'),
        threePointFieldGoalsMadeGame: this.getHighestRecord(game, 'fgm3'),
        threePointFieldGoalsAttemptsGame: this.getHighestRecord(game, 'fga3'),
        freeThrowsMadeGame: this.getHighestRecord(game, 'ftm'),
        freeThrowAttemptsGame: this.getHighestRecord(game, 'fta'),
        offensiveReboundsGame: this.getHighestRecord(game, 'oreb'),
        defensiveReboundsGame: this.getHighestRecord(game, 'dreb'),
        totalReboundsGame: this.getHighestRecord(game, 'treb'),
        assistsGame: this.getHighestRecord(game, 'ast'),
        stealsGame: this.getHighestRecord(game, 'stl'),
        blocksGame: this.getHighestRecord(game, 'blk'),
        minutesGame: this.getHighestRecord(game, 'min')
      }
    },
    getHighestRecord (game, categoryKey) {
      let highestValue = 0
      let highestValueHappenTimes = 0
      let targetGame = null
      for (let i = 0; i < game.length; i++) {
        if (parseInt(game[i].stats[categoryKey]) > highestValue) {
          // reset happen times because game change
          highestValueHappenTimes = 1
          highestValue = parseInt(game[i].stats[categoryKey])
          targetGame = game[i]
        } else if (parseInt(game[i].stats[categoryKey]) === highestValue) {
          targetGame = game[i]
          highestValueHappenTimes++
        }
        game[i].stats.highestValueHappenTimes = highestValueHappenTimes
      }
      return targetGame
    },
    getHighestValueHappenTimes (value, categoryKey) {
      const games = this.gamesRecordPlayerInCleared.filter(game => game.stats[categoryKey] === value)
      return games.length
    }
  }
}
</script>

<style scoped>

thead {
  color: white;
  background: #0b5ed7;
}

</style>
